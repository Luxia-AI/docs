# Known Issues and Next Steps

This file tracks implementation-grounded risks after the methodology refresh.

## Current Known Issues

1. Multi-path delivery complexity
- Result delivery can occur through direct HTTP, callback fallback, or Kafka consume/publish path.
- Behavior is resilient but incident tracing can be non-trivial.

2. Heuristic threshold sensitivity
- Ranking and trust gates rely on many tunable thresholds.
- Small configuration shifts can alter stop behavior and verdict confidence.

3. External dependency volatility
- Search providers, source sites, and scraping surfaces can change unpredictably.
- This affects corrective-loop yield and latency tails.

4. Store asymmetry under partial failures
- Best-effort ingestion means VDB and KG can briefly diverge if one path fails.

5. LLM degradation/fallback behavior
- Quota hardening and fallback preserve availability, but quality can drop when non-critical calls are skipped.

6. Infra definition gap
- `infra/k8s/*` and `infra/terraform/main.tf` remain placeholders.

## High-Impact Risks

1. Calibration drift
- Confidence can become over- or under-conservative if calibration artifacts are stale relative to model/policy changes.

2. Coverage overfitting
- Strict anchor/admissibility policies can under-retrieve semantically valid paraphrases.

3. Comparative/numeric edge behavior
- Deterministic override patterns are robust for many cases but still heuristic and may miss implicit evidence forms.

## Priority Next Steps

1. Formal schema versioning and contract tests
- Introduce explicit schema versions for Socket, callback, and worker payloads.
- Add CI-level contract snapshot checks.

2. Threshold governance
- Centralize ranking/trust/verdict thresholds with versioned policy bundles.
- Add regression gates for class-wise recall and calibration metrics.

3. Corrective-loop observability hardening
- Add dashboards and alerts for: zero-fact rounds, repeated query drift, callback failure rate, fallback-completed rate.

4. Storage consistency checks
- Add scheduled consistency probes for VDB/KG ingestion parity and stale URL skip-set behavior.

5. Deployment maturity
- Replace placeholder k8s/terraform with concrete, validated manifests or remove placeholders to reduce ambiguity.

## Near-Term Roadmap (Execution Order)

1. Contract formalization and CI contract diffing.
2. Threshold + calibration governance with reproducible evaluation runs.
3. Corrective-loop telemetry SLOs and alerting.
4. Storage consistency probes.
5. Deployment profile completion.

## Related Methodology Sections

- [methodology/04-ranking-trust-and-corrective-loop.md](./methodology/04-ranking-trust-and-corrective-loop.md)
- [methodology/05-verdict-synthesis.md](./methodology/05-verdict-synthesis.md)
- [methodology/06-failure-modes-and-tradeoffs.md](./methodology/06-failure-modes-and-tradeoffs.md)

Last verified against code: March 2, 2026
