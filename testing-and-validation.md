# Testing and Validation

This document defines the validation strategy for the current hybrid corrective RAG system.

## Validation Objectives

1. Contract correctness
- Ensure HTTP, Socket.IO, callback, and Kafka payload schemas remain stable.

2. Methodology correctness
- Ensure retrieval/ranking/trust/verdict stages behave according to current architecture.

3. Regression resistance
- Detect polarity drift, confidence miscalibration, KG signal collapse, and stage event regressions.

4. Operational confidence
- Validate realtime delivery, failure fallback behavior, and observability instrumentation.

## Test Surfaces

### Service and contract tests

- `socket-hub/tests/*`
  - auth/room flow
  - internal callback handling
  - Kafka integration behavior
- `dispatcher/tests/*`
  - request schema and routing
  - integration flow
- `control-plane-api/tests/*`
  - API behavior and role-scoped contract checks

### Worker pipeline tests

- retrieval and ranking:
  - `worker/tests/test_retrieval_phase.py`
  - `worker/tests/test_hybrid_rank.py`
  - `worker/tests/test_hybrid_rank_regression.py`
  - `worker/tests/test_ranking_phase_contradiction.py`
- trust and adaptive policy:
  - `worker/tests/test_trust_ranker.py`
  - `worker/tests/test_adaptive_trust_policy.py`
  - `worker/tests/test_subclaim_coverage_contract.py`
- verdict synthesis and policy:
  - `worker/tests/test_verdict_v2_engine.py`
  - `worker/tests/test_verdict_v2_policy.py`
  - `worker/tests/test_verdict_reconciliation.py`
  - `worker/tests/test_logic_overrides.py`
  - `worker/tests/test_confidence_calibration.py`
- pipeline and stage lifecycle:
  - `worker/tests/test_pipeline_integration.py`
  - `worker/tests/test_main_stage_events.py`
  - `worker/tests/test_worker_stage_callback.py`

## Recommended Validation Commands

### Full backend contract pass

```bash
pytest -q socket-hub/tests dispatcher/tests control-plane-api/tests
```

### Worker core methodology pass

```bash
pytest -q \
  worker/tests/test_retrieval_phase.py \
  worker/tests/test_hybrid_rank.py \
  worker/tests/test_hybrid_rank_regression.py \
  worker/tests/test_adaptive_trust_policy.py \
  worker/tests/test_trust_ranker.py \
  worker/tests/test_verdict_v2_engine.py \
  worker/tests/test_verdict_reconciliation.py \
  worker/tests/test_logic_overrides.py \
  worker/tests/test_confidence_calibration.py \
  worker/tests/test_main_stage_events.py
```

### Storage-aware and integration-heavy checks

```bash
pytest -q \
  worker/tests/test_pipeline_with_real_storage.py \
  worker/tests/test_pipeline_integration.py \
  worker/tests/test_pipeline_actual.py
```

### Evaluation utilities (offline analysis)

```bash
python evaluation/analyze_runs.py --runs evaluation/runs --out evaluation/artifacts/metrics.json
python evaluation/train_calibrator.py --runs evaluation/runs --out evaluation/artifacts/calibrator_params.json
python evaluation/retrieval_health_check.py
```

## Research-Grade QA Scenarios

1. Contract drift scenario
- Submit a claim with `client_claim_id`, verify `worker_update` preserves correlation IDs and includes verdict/trust fields.

2. Stage lifecycle scenario
- Verify stage progression appears in valid order and `completed` is emitted exactly once.

3. Cache-fast-path scenario
- Use a claim with strong indexed evidence and confirm `completed_from_cache` behavior.

4. Corrective-loop scenario
- Use sparse claim context and verify incremental search->extract->ingest->rerank loop behavior.

5. Contradiction-dominance scenario
- Verify strictness/override logic can force/refine verdict under strong contradictory evidence.

6. Confidence calibration scenario
- Validate calibrated confidence behavior under low coverage/high contradiction conditions.

## Acceptance Criteria

1. All contract and lifecycle tests pass in CI baseline.
2. No schema regressions in dispatch/worker/socket payloads.
3. Stage callbacks remain aligned with pipeline execution milestones.
4. Trust and verdict diagnostics remain present in completed payloads.
5. No unexpected fallback-completed spikes under normal test runs.

## Methodology Cross-Reference

- Architecture: [methodology/01-system-architecture.md](./methodology/01-system-architecture.md)
- Pipeline stages: [methodology/02-pipeline-stage-decomposition.md](./methodology/02-pipeline-stage-decomposition.md)
- Retrieval/ingestion: [methodology/03-retrieval-and-ingestion.md](./methodology/03-retrieval-and-ingestion.md)
- Ranking/trust: [methodology/04-ranking-trust-and-corrective-loop.md](./methodology/04-ranking-trust-and-corrective-loop.md)
- Verdict logic: [methodology/05-verdict-synthesis.md](./methodology/05-verdict-synthesis.md)

Last verified against code: March 2, 2026
