# Health-Only Hybrid RAG Refactor Plan (Deterministic Polarity + Calibration v2)

Supplemental plan note: this file is a focused refactor proposal. For the current implemented methodology and contracts, use `docs/methodology/README.md` and linked sections.

## 1. Problem Summary
- Polarity collapse toward TRUE-like outputs reduced FALSE and UNVERIFIABLE recall.
- Contradiction evidence admission was too sparse in decisive paths.
- Confidence was frequently miscalibrated, with high wrong-confidence.
- KG zero-signal rounds correlated with degraded verdict quality.
- Trust sufficiency and verdict locks created policy conflicts.
- Repeat instability and corrective-loop latency tails reduced reliability.

## 2. Architectural Changes
1. Deterministic stance aggregation:
   - Per evidence row `support/refute/neutral` scoring.
   - Stage-1 contradiction candidacy and stage-2 verification.
2. Single posterior policy:
   - Compute `P(TRUE)`, `P(FALSE)`, `P(UNVERIFIABLE)` from evidence masses.
   - Verdict selected by posterior argmax (v2 path).
3. Confidence from evidence geometry:
   - Margin, sufficiency, agreement, entropy.
   - Offline class-conditional calibration support.
4. Trust as a soft signal:
   - `trust_support`, `trust_contradict`, `trust_uncertain`, `admissibility_rate`.
   - No hard verdict forcing in v2 policy path.
5. KG resilience:
   - Fallback retrieval pass on empty KG signal using normalized/expanded entities.
6. Corrective loop determinism and observability:
   - Information-gain telemetry.
   - Deterministic query cache in evaluation mode.

## 3. File-by-File Implementation

### Verdict v2 policy and stance pipeline
- `worker/app/services/verdict/v2/types.py`
- `worker/app/services/verdict/v2/entailment.py` (new)
- `worker/app/services/verdict/v2/stance_pipeline.py` (new)
- `worker/app/services/verdict/v2/posterior.py` (new)
- `worker/app/services/verdict/v2/policy.py` (new)
- `worker/app/services/verdict/v2/calibration.py`
- `worker/app/services/verdict/v2/__init__.py`
- `worker/app/services/verdict/verdict_generator.py`

### Ranking/retrieval integration
- `worker/app/services/corrective/pipeline/ranking_phase.py`
- `worker/app/services/ranking/hybrid_ranker.py`
- `worker/app/services/corrective/pipeline/retrieval_phase.py`
- `worker/app/services/corrective/pipeline/__init__.py`
- `worker/app/services/corrective/trusted_search.py`

### Evaluation and calibration tooling
- `evaluation/metrics_utils.py` (new)
- `evaluation/analyze_runs.py` (new)
- `evaluation/train_calibrator.py` (new)
- `evaluation/README.md`
- `evaluation/artifacts/calibrator_params.json` (generated artifact)

## 4. Instrumentation Added
- `refute_candidate_count_stage1`
- `refute_verified_count_stage2`
- `refutes_admission_rate`
- `kg_zero_signal`
- `kg_fallback_triggered`
- `stance_scores` (`support_mass`, `refute_mass`, `neutral_mass`)
- `evidence_sufficiency`
- `agreement_score`
- `retrieval_entropy`
- `pipeline_diagnostics_v2` (`stop_reason`, `gain_estimate`, `zero_extraction_rounds`, KG flags)
- `trust_snapshot_v2`

## 5. Acceptance Targets
- FALSE recall `>= 0.55`
- UNVERIFIABLE recall `>= 0.40`
- Macro-ECE `<= 0.12`
- Repeat flip rate `< 0.15`
- REFUTES admission rate materially above prior baseline (~3%)
- p95 latency `<= 120s`
- No claim-specific keyword/entity special-casing

## 6. Validation Commands
```bash
pytest -q worker/tests/test_hybrid_rank.py worker/tests/test_ranking_grading_integration.py
pytest -q worker/tests/test_verdict_v2_engine.py worker/tests/test_confidence_calibration.py
python evaluation/analyze_runs.py --runs evaluation/runs --out evaluation/artifacts/metrics.json
python evaluation/train_calibrator.py --runs evaluation/runs --out evaluation/artifacts/calibrator_params.json
```

## 7. Rollout
1. `VERDICT_ENGINE_V2_SHADOW=true` (shadow-only decision parity telemetry).
2. Evaluate deltas on accuracy, class recall, ECE, flip rate, and p95 latency.
3. Canary progression `10% -> 25% -> 50% -> 100%`.
4. Rollback triggers:
   - Macro-ECE regression `> 0.03`
   - FALSE or UNVERIFIABLE recall drop `> 0.10`
   - Sustained p95 latency `> 120s`

## 8. Assumptions
- Deployment is health-only; no runtime out-of-scope branching required.
- No claim/entity-specific rules are introduced.
- If NLI model is unavailable, contradiction verifier falls back conservatively to neutral-biased scoring with explicit telemetry.
