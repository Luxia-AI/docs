# Methodology Figure Pack (F01-F56)

This directory contains an exhaustive, publication-ready figure suite using IEEE/ACM style and dual Mermaid+Spec contracts.

## Files
- [00-figure-style-guide.md](./00-figure-style-guide.md)
- [01-method-overview-pack.md](./01-method-overview-pack.md)
- [02-intake-and-claim-understanding-pack.md](./02-intake-and-claim-understanding-pack.md)
- [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md)
- [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md)
- [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md)
- [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md)
- [07-latency-throughput-and-cost-pack.md](./07-latency-throughput-and-cost-pack.md)
- [08-generalization-and-safety-pack.md](./08-generalization-and-safety-pack.md)
- [09-ablation-and-sensitivity-pack.md](./09-ablation-and-sensitivity-pack.md)
- [10-reproducibility-and-governance-pack.md](./10-reproducibility-and-governance-pack.md)
- [appendix-figure-variants.md](./appendix-figure-variants.md)

## Figure-to-Paper-Section Matrix

| Figure | Title | Paper Section | Pack | Main/Appendix | Column |
|---|---|---|---|---|---|
| F01 | End-to-end pipeline architecture | Methodology: System Architecture | [01-method-overview-pack.md](./01-method-overview-pack.md) | Main | 2-column |
| F02 | Layered subsystem map | Methodology: System Architecture | [01-method-overview-pack.md](./01-method-overview-pack.md) | Main | 2-column |
| F03 | Data/control plane separation | Methodology: System Architecture | [01-method-overview-pack.md](./01-method-overview-pack.md) | Main | 1-column |
| F04 | Online vs offline evaluation loop | Methodology: Evaluation Protocol | [01-method-overview-pack.md](./01-method-overview-pack.md) | Main | 1-column |
| F05 | Research contribution map | Introduction / Contributions | [01-method-overview-pack.md](./01-method-overview-pack.md) | Main | 1-column |
| F06 | Claim parsing finite-state view | Methodology: Claim Understanding | [02-intake-and-claim-understanding-pack.md](./02-intake-and-claim-understanding-pack.md) | Main | 1-column |
| F07 | Sub-claim decomposition tree | Methodology: Claim Understanding | [02-intake-and-claim-understanding-pack.md](./02-intake-and-claim-understanding-pack.md) | Main | 2-column |
| F08 | Entity-predicate-object extraction flow | Methodology: Claim Understanding | [02-intake-and-claim-understanding-pack.md](./02-intake-and-claim-understanding-pack.md) | Main | 1-column |
| F09 | Negation/quantifier polarity map | Methodology: Claim Understanding | [02-intake-and-claim-understanding-pack.md](./02-intake-and-claim-understanding-pack.md) | Main | 1-column |
| F10 | Temporal qualifier handling logic | Methodology: Claim Understanding | [02-intake-and-claim-understanding-pack.md](./02-intake-and-claim-understanding-pack.md) | Appendix | 1-column |
| F11 | Hybrid retrieval orchestration sequence | Methodology: Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Main | 2-column |
| F12 | VDB retrieval decision surface | Methodology: Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Appendix | 1-column |
| F13 | KG traversal and expansion graph | Methodology: Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Main | 1-column |
| F14 | Retrieval fusion DAG | Methodology: Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Main | 1-column |
| F15 | Query generation lattice (support/refute tracks) | Methodology: Corrective Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Main | 2-column |
| F16 | Corrective loop state machine | Methodology: Corrective Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Main | 1-column |
| F17 | Query budget and stop-criterion flow | Methodology: Corrective Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Appendix | 1-column |
| F18 | Source acquisition funnel | Methodology: Retrieval | [03-retrieval-and-corrective-pack.md](./03-retrieval-and-corrective-pack.md) | Main | 1-column |
| F19 | Candidate ranking pipeline | Methodology: Ranking | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 2-column |
| F20 | Stance-aware reranking matrix | Methodology: Ranking | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 1-column |
| F21 | Trust score decomposition | Methodology: Trust Policy | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 1-column |
| F22 | Evidence admissibility gate | Methodology: Evidence Validation | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 1-column |
| F23 | Alignment scoring anatomy | Methodology: Evidence Validation | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 1-column |
| F24 | Contradiction admission path | Methodology: Ranking | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 1-column |
| F25 | Evidence diversity vs agreement map | Methodology: Trust Policy | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Appendix | 1-column |
| F26 | Neutral-only evidence detection flow | Methodology: Evidence Validation | [04-ranking-trust-and-evidence-quality-pack.md](./04-ranking-trust-and-evidence-quality-pack.md) | Main | 1-column |
| F27 | Internal verdict policy graph | Methodology: Verdict Synthesis | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 2-column |
| F28 | Binary projection logic graph | Methodology: Verdict Synthesis | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 1-column |
| F29 | Policy trace decision table graphic | Methodology: Verdict Synthesis | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 1-column |
| F30 | Truth score mass-balance equation diagram | Methodology: Verdict Synthesis | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 1-column |
| F31 | Confidence composition chart | Methodology: Calibration | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 1-column |
| F32 | Class-probability coherence projection | Methodology: Calibration | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Appendix | 1-column |
| F33 | Abstain reason taxonomy map | Methodology: Verdict Synthesis | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 1-column |
| F34 | Confidence calibration pipeline | Methodology: Calibration | [05-verdict-policy-and-calibration-pack.md](./05-verdict-policy-and-calibration-pack.md) | Main | 1-column |
| F35 | Metrics stack map (Acc/F1/ECE/Brier/NLL) | Evaluation | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Main | 1-column |
| F36 | Version progression timeline | Evaluation | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Main | 2-column |
| F37 | Error taxonomy Sankey | Failure Analysis | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Main | 2-column |
| F38 | Failure propagation chain diagram | Failure Analysis | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Main | 1-column |
| F39 | Alignment-zero cohort analysis | Failure Analysis | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Main | 1-column |
| F40 | Confident-wrong decomposition | Failure Analysis | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Appendix | 1-column |
| F41 | KG/VDB imbalance dashboard layout | Failure Analysis | [06-evaluation-and-failure-analysis-pack.md](./06-evaluation-and-failure-analysis-pack.md) | Main | 1-column |
| F42 | Stage-wise latency waterfall | System Performance | [07-latency-throughput-and-cost-pack.md](./07-latency-throughput-and-cost-pack.md) | Main | 1-column |
| F43 | p50/p95/p99 profile curves | System Performance | [07-latency-throughput-and-cost-pack.md](./07-latency-throughput-and-cost-pack.md) | Main | 1-column |
| F44 | Throughput vs quality frontier | System Performance | [07-latency-throughput-and-cost-pack.md](./07-latency-throughput-and-cost-pack.md) | Main | 2-column |
| F45 | Corrective-loop cost curve | System Performance | [07-latency-throughput-and-cost-pack.md](./07-latency-throughput-and-cost-pack.md) | Appendix | 1-column |
| F46 | No-hardcoding compliance checklist graphic | Generalization/Safety | [08-generalization-and-safety-pack.md](./08-generalization-and-safety-pack.md) | Main | 1-column |
| F47 | Domain-boundary enforcement flow | Generalization/Safety | [08-generalization-and-safety-pack.md](./08-generalization-and-safety-pack.md) | Main | 1-column |
| F48 | Hallucination containment controls | Generalization/Safety | [08-generalization-and-safety-pack.md](./08-generalization-and-safety-pack.md) | Main | 1-column |
| F49 | Robustness under low-evidence conditions | Generalization/Safety | [08-generalization-and-safety-pack.md](./08-generalization-and-safety-pack.md) | Main | 1-column |
| F50 | Retrieval weight ablation matrix | Ablations | [09-ablation-and-sensitivity-pack.md](./09-ablation-and-sensitivity-pack.md) | Main | 1-column |
| F51 | Trust-threshold sensitivity curves | Ablations | [09-ablation-and-sensitivity-pack.md](./09-ablation-and-sensitivity-pack.md) | Main | 1-column |
| F52 | Calibration method comparison panel | Ablations | [09-ablation-and-sensitivity-pack.md](./09-ablation-and-sensitivity-pack.md) | Main | 1-column |
| F53 | Query-strategy ablation map | Ablations | [09-ablation-and-sensitivity-pack.md](./09-ablation-and-sensitivity-pack.md) | Appendix | 1-column |
| F54 | Artifact lineage graph | Reproducibility | [10-reproducibility-and-governance-pack.md](./10-reproducibility-and-governance-pack.md) | Main | 1-column |
| F55 | Experiment protocol schedule | Reproducibility | [10-reproducibility-and-governance-pack.md](./10-reproducibility-and-governance-pack.md) | Main | 1-column |
| F56 | Daily/weekly regression governance chart | Governance | [10-reproducibility-and-governance-pack.md](./10-reproducibility-and-governance-pack.md) | Main | 1-column |

## Figure-to-Repo-Signal Matrix

| Figure | Primary Repo Signals / Artifacts |
|---|---|
| F01 | worker/app/main.py debug block; worker pipeline stage events |
| F02 | docs/system-overview.md; docs/interfaces-and-contracts.md |
| F03 | dispatcher/socket-hub logs; control-plane API contracts |
| F04 | evaluation/runs/*; evaluation/artifacts/metrics.json |
| F05 | docs/methodology/*.md synthesis |
| F06 | worker corrective pipeline claim preprocessing logs |
| F07 | final_payload.claim_breakdown |
| F08 | corrective pipeline phase outputs |
| F09 | verdict_generator quantifier signals |
| F10 | evidence metadata + claim temporal tokens |
| F11 | retrieval phase outputs; debug.generated_queries |
| F12 | VDB retrieval filter logs |
| F13 | KG retrieval outputs |
| F14 | retrieval_phase semantic_dedup_count + kg_with_score |
| F15 | debug.generated_queries + corrective loop logs |
| F16 | debug.query_budget + debug.stop_reason |
| F17 | pipeline stop decision logs |
| F18 | scraper/fact_extractor phase outputs |
| F19 | ranking_phase top_k_selected |
| F20 | ranking scores + evidence_map relevance |
| F21 | adaptive_trust_policy logs + payload.trust_post |
| F22 | evidence_map alignment/validation fields |
| F23 | debug.alignment_score + evidence-level features |
| F24 | refute_pipeline_stats |
| F25 | trust_snapshot/debug.trust_gate |
| F26 | debug.evidence_stance_distribution |
| F27 | final_payload policy fields |
| F28 | policy_trace deterministic_binary_projection |
| F29 | debug.verdict_policy_path + policy_trace |
| F30 | final_payload mass fields |
| F31 | final_payload confidence + sufficiency_score |
| F32 | class_probs_resync_reason |
| F33 | final_payload abstain_reason + guard reasons |
| F34 | calibration_meta + confidence outputs |
| F35 | evaluation/artifacts/metrics.json |
| F36 | analyze_runs by_version outputs |
| F37 | failure diagnostics + debug fields |
| F38 | stage_events + policy trace |
| F39 | new diagnostics in analyze_runs |
| F40 | evaluation samples + confidence fields |
| F41 | metrics + debug vector_hits/kg_hits |
| F42 | stage_events timestamps + elapsed_seconds |
| F43 | evaluation run latencies |
| F44 | evaluation + deployment metrics |
| F45 | debug.query_budget + stop_reason + metrics deltas |
| F46 | code audit checklist + module scan |
| F47 | pipeline scope checks + source policy |
| F48 | verdict/evidence attribution outputs |
| F49 | final payload evidence_count + confidence + abstain |
| F50 | ablation experiments |
| F51 | threshold sweep runs |
| F52 | calibration experiment logs |
| F53 | query strategy experiment runs |
| F54 | evaluation/runs + artifacts lineage |
| F55 | protocol docs + evaluation workflow |
| F56 | daily reports + regression checks |

## Core Submission Set (Page-Limited)
Recommended core set: F01, F04, F07, F11, F15, F19, F21, F24, F27, F28, F31, F34, F36, F37, F39, F41, F44, F46, F48, F54.

## Compliance Notes
- IDs F01-F56 are defined once each.
- All sections include required figure-spec fields.
- All figures are health-domain-general and claim-agnostic.

