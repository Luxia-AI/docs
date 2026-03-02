# Ranking Fix Notes

Supplemental note: this file captures a targeted ranking remediation slice. For current end-to-end methodology context, use `docs/methodology/README.md` first.

Changes to `worker/app/services/ranking/hybrid_ranker.py` to fix 15 failing
tests across `test_hybrid_rank.py` (9 failures) and
`test_ranking_grading_integration.py` (6 failures).

## Root Causes Identified

### RC1: Aggressive text-dependent filters when no query context

When `query_text` was empty or absent, `claim_overlap` evaluated to 0 for all
candidates. The admission filter `claim_overlap < 0.15` then eliminated nearly
everything. KG-only candidates (with `sem_s=0`) were doubly penalized by the
`sem_s < 0.20` filter.

### RC2: Multiplicative penalty chain compressing scores toward zero

The original code applied cascading multiplicative penalties
(`*= 0.30`, `*= 0.70`, `*= 0.55`, etc.) that compounded catastrophically.
Combined with the aggressive 0.65/0.35 blending ratio between base score and
stance priority, candidates with even minor misalignments collapsed to near-zero.

### RC3: KG candidates systematically eliminated

Even strong KG candidates were killed because:

- Claim overlap and anchor requirements were undefined without query text.
- No KG retention policy existed to rescue dropped KG evidence.
- No authority credibility bonus existed to lift KG from authoritative sources.

## File-by-File Changes

### `worker/app/services/ranking/hybrid_ranker.py`

The `hybrid_rank()` function (lines 477-999) was rewritten. Helper functions
above line 477 were NOT changed.

**Key changes:**

1. **Query context gating** (`has_query_context` flag, line 526):
   All text-dependent penalties and admission filters are gated on
   `bool(query_text and len(query_text.strip()) > 3)`. Without context,
   only absolute noise is dropped.

2. **Authority credibility bonus** (lines 648-654):
    - `+0.30` for `cred >= 0.90` (tier-1 authorities: WHO, NIH, CDC, etc.)
    - `+0.10` for `cred >= 0.75`
      This is general, not claim-specific.

3. **Bounded additive penalties** (lines 681-723):
   Replaced multiplicative penalty chain with a penalty collection system.
   Each misalignment contributes a penalty strength; the worst dominates with
   diminishing returns on additional penalties (0.25 factor). Total penalty is
   capped at 65%.

4. **Softer blending** (line 771):
   Changed from `0.65 * base + 0.35 * priority` to
   `0.95 * base + 0.05 * priority`. The priority now acts primarily as a
   tiebreaker rather than a dominating term.

5. **`sem_score` output change** (line 780):
   Output `sem_score` is now the weighted component (`w_sem * sem_s`) instead
   of the raw normalized score. `sem_score_raw` preserves the original value.

6. **Two-pass architecture** (lines 593-906):
   Score all candidates into `all_scored` first, then apply admission filters
   in a separate pass. This enables the KG retention policy to rescue dropped
   KG candidates.

7. **KG minimum retention** (lines 908-930):
   If KG candidates existed in input but none survived filters, the best KG
   candidate is re-admitted if it has adequate score (`kg_raw >= 0.30` or
   `final_score >= 0.10`).

8. **Deterministic sort** (lines 937-945):
   Multi-level sort key: `(-final_score, -credibility, -support_score,
-recency, statement)`. Alphabetical statement as final tie-breaker.

9. **Debug diagnostics** (line 528, 811-820):
   Behind `HYBRID_RANK_DEBUG=1` environment variable. Logs per-candidate
   scoring breakdown.

10. **New output fields**: `credibility_bonus`, `recency_score`.
    No fields were removed.

### `worker/tests/test_hybrid_rank_regression.py` (NEW)

23 regression tests in 4 categories:

- `TestKGOnlyNonEmpty` (5 tests): KG-only input yields non-empty ranked output
- `TestMixedPreservesKG` (4 tests): Mixed VDB+KG preserves KG candidates
- `TestDedupCorrectness` (4 tests): Dedup merges correctly, preserves unique
- `TestScoreSanity` (10 tests): Score bounds, spread, determinism, output fields

### `evaluation/retrieval_health_check.py` (NEW)

Parses evaluation run artifacts and computes per-claim retrieval health metrics.
Generates alerts for:

- `KG_DROPPED`: KG candidates present but none in final evidence
- `KG_ZERO_SIGNAL`: Many KG candidates but zero signal count
- `LOW_KG_MASS`: KG mass ratio below 5%
- `LOW_TOP_SCORE`: Top ranking score below 0.20
- `SCORE_COMPRESSION`: Score spread below 0.05 across 3+ items
- `NO_EVIDENCE`: No evidence items in output

Outputs:

- `evaluation/retrieval_health_report.json`
- `evaluation/retrieval_health_report.md`

## How to Run

```bash
# Run original ranking tests
python -m pytest worker/tests/test_hybrid_rank.py -v

# Run integration tests
python -m pytest worker/tests/test_ranking_grading_integration.py -v

# Run regression tests
python -m pytest worker/tests/test_hybrid_rank_regression.py -v

# Run all ranking tests together
python -m pytest worker/tests/test_hybrid_rank.py \
  worker/tests/test_ranking_grading_integration.py \
  worker/tests/test_hybrid_rank_regression.py -v

# Run health check
python evaluation/retrieval_health_check.py
```

## Metrics to Watch

| Metric               | Healthy Range | Alert If |
| -------------------- | ------------- | -------- |
| KG conversion rate   | >15%          | <5%      |
| KG mass ratio        | >10%          | <5%      |
| Top ranking score    | >0.40         | <0.20    |
| Score spread (top-k) | >0.05         | <0.05    |
| KG dropped rate      | <10%          | >25%     |

## No Claim-Specific Hacks Confirmation

All fixes are generalizable:

- No hardcoded claim text, disease names, or treatment keywords
- Authority bonus applies uniformly to any trusted domain (WHO, NIH, CDC, etc.)
- Penalty bounds and blending ratios are claim-agnostic
- KG retention policy applies to any KG candidate, not specific entity types
- Query context gating uses length threshold (>3 chars), not content matching
