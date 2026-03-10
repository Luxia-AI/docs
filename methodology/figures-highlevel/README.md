# Final High-Level Figure Set (H01-H20)

This directory contains the standalone, publication-facing final 20 methodology figures for the main paper.

Deep technical/appendix suite: [../figures/README.md](../figures/README.md)

## Main Paper Figure Sequence
`H01 → H02 → H03 → H04 → H05 → H06 → H07 → H08 → H09 → H10 → H11 → H12 → H13 → H14 → H15 → H16 → H17 → H18 → H19 → H20`

## Files
- [00-style-and-contract.md](./00-style-and-contract.md)
- [01-core-methodology-pack.md](./01-core-methodology-pack.md)
- [02-retrieval-ranking-trust-pack.md](./02-retrieval-ranking-trust-pack.md)
- [03-verdict-calibration-evaluation-pack.md](./03-verdict-calibration-evaluation-pack.md)

## Figure-to-Paper-Section Matrix

| Figure | Paper Section | Main/Appendix Candidate |
|---|---|---|
| H01 | Methods / System Overview | Main body |
| H02 | Methods / Runtime Flow | Main body |
| H03 | Methods / Claim Understanding | Main body |
| H04 | Methods / Retrieval | Main body |
| H05 | Methods / Corrective Retrieval | Main body |
| H06 | Methods / Evidence Validation | Main body |
| H07 | Methods / Ranking and Trust | Main body |
| H08 | Methods / Explainability | Main body |
| H09 | Methods / Query Generation | Main body |
| H10 | Methods / Hybrid Retrieval | Main body |
| H11 | Methods / Ranking | Main body |
| H12 | Methods / Evidence Policy | Appendix candidate |
| H13 | Methods / Trust Policy | Main body |
| H14 | Methods / Efficiency | Appendix candidate |
| H15 | Methods / Verdict Synthesis | Main body |
| H16 | Methods / Output Policy | Main body |
| H17 | Methods / Confidence Modeling | Main body |
| H18 | Methods / Calibration | Main body |
| H19 | Evaluation | Main body |
| H20 | Reproducibility / Governance | Main body |

## Figure-to-Repo-Signal Matrix

| Figure | Primary Repo Signals / Fields |
|---|---|
| H01 | stage events, `final_payload` verdict and debug fields |
| H02 | control-plane/dispatcher/worker response handoff fields |
| H03 | claim entities, predicate target, quantifier/negation flags |
| H04 | `semantic_candidates_count`, `kg_candidates_count`, `kg_fallback_triggered` |
| H05 | `debug.query_budget`, `debug.stop_reason`, adaptive sufficiency logs |
| H06 | alignment fields and evidence gating signals in `evidence_map` |
| H07 | ranking phase outputs, `trust_post`, diversity/agreement metrics |
| H08 | `claim_breakdown`, `evidence_attribution`, `evidence_used_ids` |
| H09 | `debug.generated_queries`, query reformulation logs |
| H10 | `kg_utilization_ratio`, `kg_zero_signal_rate`, debug hit counts |
| H11 | `refute_candidate_count_stage1`, `refute_verified_count_stage2` |
| H12 | stance distribution, `abstain_reason`, internal vs binary verdict |
| H13 | trust-gate + directional mass signals |
| H14 | stage latencies and per-run elapsed seconds |
| H15 | policy trace, guard reasons, support/contradict/neutral masses |
| H16 | deterministic projection path, `binary_collapse_reason` |
| H17 | `truth_score_binary`, `confidence`, `sufficiency_score` |
| H18 | `class_probs`, `calibrated_confidence`, `calibration_meta` |
| H19 | `evaluation/artifacts/metrics.json` core metrics |
| H20 | regression workflow artifacts and version comparisons |
