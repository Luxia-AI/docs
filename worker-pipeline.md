# Worker Pipeline Deep Dive

`worker` exposes `POST /worker/verify` and returns a normalized verification payload.

## Execution Layers

1. API layer (`worker/app/main.py`)
2. Pipeline orchestration (`worker/app/services/corrective/pipeline/__init__.py`)
3. Retrieval and ranking stack
4. Verdict generation (`worker/app/services/verdict/verdict_generator.py`)

## Request and Response Core

Request model (`VerifyRequest`) includes:

- `job_id`, `claim`, optional `room_id`, `source`
- `domain` (default `general`)
- `top_k` (default 5, range 1..20)

Response includes:

- final verdict fields
- ranking and trust metrics
- evidence and evidence_map summaries
- pipeline/data-source metadata

## Corrective Pipeline Behavior

Implemented orchestration emphasizes quota efficiency:

- retrieval-first from available stores
- trust evaluation before expensive web search expansion
- incremental search/query processing when confidence is insufficient
- early stop when threshold criteria are met

Primary collaborators:

- extraction: facts, entities, relations
- stores: Pinecone (VDB), Neo4j (KG)
- retrieval: semantic + structural
- ranking: hybrid scoring and trust policy

## Trust and Ranking

Key runtime signals in payload:

- `top_ranking_score`, `avg_ranking_score`
- `vdb_signal_count`, `kg_signal_count`
- `vdb_signal_sum_top5`, `kg_signal_sum_top5`
- `trust_policy_mode`, `trust_metric_name`, `trust_metric_value`
- `trust_threshold`, `trust_threshold_met`
- `coverage`, `diversity`, `num_subclaims`

## Verdict Generation

`VerdictGenerator` is responsible for:

- verdict classes: `TRUE`, `FALSE`, `PARTIALLY_TRUE`, `UNVERIFIABLE`
- rationale and key findings
- claim breakdown and evidence map
- truthfulness percentage and confidence calibration

The generator includes safeguards for:

- insufficient evidence handling
- contradiction detection and stance-aware aggregation
- comparative/numeric claim edge handling paths

## Fallback Mode

If pipeline execution fails at runtime:

- worker returns fallback-completed payload
- mock verdict logic is applied from claim text patterns
- service availability is preserved

## Key Files

- `worker/app/main.py`
- `worker/app/services/corrective/pipeline/__init__.py`
- `worker/app/services/ranking/hybrid_ranker.py`
- `worker/app/services/verdict/verdict_generator.py`

Last verified against code: February 13, 2026
