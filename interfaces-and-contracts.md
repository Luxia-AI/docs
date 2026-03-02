# Interfaces and Contracts

This document defines the current, code-verified contracts across HTTP, Socket.IO, callback, and Kafka boundaries.

## Contract Scope

- Ingress/auth and realtime transport (`socket-hub` + `control-plane-api`)
- Dispatch orchestration (`dispatcher`)
- Verification compute (`worker`)
- Callback transport (`/internal/dispatch-stage`, `/internal/dispatch-result`)
- Optional Kafka mirror path

For architecture rationale and internals, see [methodology/README.md](./methodology/README.md).

## HTTP API Surface

### socket-hub

- `GET /` service liveness payload
- `GET /healthz` health check
- `GET /ready` readiness check
- `GET /metrics` Prometheus metrics
- `WebSocket /ws` echo/debug websocket
- `POST /internal/dispatch-result` internal result callback (token gated when configured)
- `POST /internal/dispatch-stage` internal stage callback (token gated when configured)

### dispatcher

- `GET /` service liveness payload
- `GET /healthz` health check
- `GET /dispatch/test` dispatch counter probe
- `GET /metrics` Prometheus metrics
- `POST /dispatch/submit` worker dispatch endpoint

### worker

- `GET /` service liveness payload
- `GET /healthz` health check
- `GET /worker/test` completion counter probe
- `GET /metrics` Prometheus metrics
- `POST /worker/verify` claim verification endpoint

### control-plane-api (contract-relevant subset)

- `GET /` service liveness payload
- `GET /healthz` health check
- `POST /v1/socket/authorize` join/post authorization
- `POST /v1/socket/verify-room-secret` room password verification
- `POST /v1/client-registrations` client/room registration bootstrap
- `POST /v1/client-registrations/{registration_id}/approve` admin approval
- `GET /v1/client/rooms` room listing by scope
- `POST /v1/client/rooms` room creation
- `POST /v1/client/rooms/{room_id}/credentials/rotate` room secret rotation

## Socket.IO Event Contract (`socket-hub`)

### Inbound events

- `join_room`
- `post_message`

### Outbound events

- `join_room_success`
- `auth_error`
- `worker_stage`
- `worker_update`

### `join_room` payload

```json
{
  "room_id": "string",
  "client_id": "string",
  "access_token": "string",
  "password": "string"
}
```

### `post_message` payload

```json
{
  "room_id": "string",
  "client_id": "string",
  "access_token": "string",
  "client_claim_id": "string",
  "claim": "string"
}
```

### `worker_stage` payload

```json
{
  "job_id": "string",
  "room_id": "string",
  "client_id": "string",
  "client_claim_id": "string",
  "claim": "string",
  "stage": "started|retrieval_done|ranking_done|search_done|extraction_done|ingestion_done|completed|error",
  "stage_payload": {},
  "timestamp": "iso8601",
  "source": "worker"
}
```

### `worker_update` lifecycle semantics

- Early status: `processing` (emitted by `socket-hub` immediately after claim acceptance)
- Final status: `completed` (normal or fallback-completed payload)
- Error status: `error` (dispatch or transport failures)

## Dispatch and Worker Payload Contracts

### `POST /dispatch/submit` request

```json
{
  "job_id": "string",
  "claim": "string",
  "room_id": "string|null",
  "client_id": "string|null",
  "client_claim_id": "string|null",
  "source": "string|null"
}
```

### `POST /dispatch/submit` response shape

```json
{
  "status": "ok",
  "service": "dispatcher",
  "job_id": "string",
  "room_id": "string|null",
  "client_id": "string|null",
  "client_claim_id": "string|null",
  "result": {}
}
```

### `POST /worker/verify` request (`VerifyRequest`)

```json
{
  "job_id": "string",
  "claim": "string",
  "room_id": "string|null",
  "client_id": "string|null",
  "client_claim_id": "string|null",
  "source": "string|null",
  "domain": "health",
  "top_k": 5
}
```

### `POST /worker/verify` response core fields

- identity: `status`, `job_id`, `room_id`, `client_id`, `client_claim_id`, `claim`
- lifecycle: `pipeline_status`, `result_status`, `timestamp`
- verdict: `verdict`, `display_verdict`, `verdict_band`, `verdict_confidence`, `calibrated_confidence`, `truthfulness_percent`
- verdict detail: `verdict_rationale`, `key_findings`, `claim_breakdown`, `evidence_map`
- retrieval/ranking/trust: `semantic_candidates_count`, `kg_candidates_count`, ranking and trust metrics
- diagnostics: `pipeline_diagnostics_v2`, `trust_snapshot_v2`, `llm`, `stage_events`

## Internal Callback Contract

### `POST /internal/dispatch-result`

- Required payload field: `room_id` (otherwise ignored)
- Optional hardening header: `x-dispatcher-token`
- Emits payload as `worker_update` to room

### `POST /internal/dispatch-stage`

- Required payload field: `room_id` (otherwise ignored)
- Optional hardening header: `x-dispatcher-token`
- Normalizes stage payload and emits `worker_stage`

## Kafka Contract (Optional Path)

Default topics:

- inbound posts: `posts.inbound`
- outbound results: `jobs.results`

Payload conventions:

- inbound post includes `job_id`, `room_id`, `claim`, and client correlation identifiers
- result message includes at minimum `status`, `job_id`, `room_id` (full worker payload when available)

## Critical Configuration Controls

- transport/auth: `CONTROL_PLANE_URL`, `SOCKETHUB_RESULT_CALLBACK_TOKEN`
- socket-hub transport: `SOCKETHUB_USE_REDIS`, `SOCKETHUB_USE_KAFKA`, `SOCKETHUB_USE_KAFKA_RESULTS`
- dispatcher transport: `ENABLE_KAFKA`, callback URL/token variables
- timeout envelope: `DISPATCH_*TIMEOUT*`, `WORKER_*TIMEOUT*`
- worker runtime: `WORKER_PRELOAD_PIPELINE`, `LUXIA_CONFIDENCE_MODE`
- llm/verdict governance: `GROQ_MAX_CALLS_PER_JOB`, `VERDICT_ENGINE_V2_*`

## Source-of-Truth Files

- `socket-hub/app/main.py`
- `socket-hub/app/sockets/manager.py`
- `dispatcher/app/main.py`
- `worker/app/main.py`
- `control-plane-api/app/main.py`
- `control-plane-api/app/routers/v1.py`

Last verified against code: March 2, 2026
