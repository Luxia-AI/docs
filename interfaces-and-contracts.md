# Interfaces and Contracts

This document captures implemented contracts across transport layers.

## HTTP Endpoints

### socket-hub

- `GET /` service status
- `GET /health` (router-level health check in `app/api/routes.py`)
- `POST /internal/dispatch-result` internal callback ingestion (token-gated when configured)

### dispatcher

- `GET /` service status
- `GET /healthz`
- `GET /dispatch/test`
- `POST /dispatch/submit`

### worker

- `GET /` service status
- `GET /healthz`
- `GET /worker/test`
- `POST /worker/verify`

## Socket.IO Events (socket-hub)

- inbound:
  - `join_room`
  - `post_message`
- outbound:
  - `join_room_success`
  - `auth_error`
  - `worker_update`

Room/auth behavior is backed by Redis via `RoomManager`.

## Kafka Topics

Configured defaults:

- `POSTS_TOPIC`: `posts.inbound`
- `RESULTS_TOPIC`: `jobs.results`
- dispatcher DLQ/logical topic support via config (`DLQ_TOPIC`)

## Cross-Service Payload Essentials

Dispatch submit payload:

- `job_id`, `claim`
- optional: `room_id`, `source`

Worker response essentials:

- `status`, `job_id`, `claim`
- verdict/rationale/truth/confidence
- trust and ranking metrics
- evidence summaries

## Critical Environment Variables

- Socket/dispatch:
  - `DISPATCHER_URL`, `WORKER_URL`
  - timeout knobs (`*_TIMEOUT_SECONDS`)
- Kafka:
  - `KAFKA_BOOTSTRAP`
  - optional SASL/SSL settings (`KAFKA_SECURITY_PROTOCOL`, username/password, retries/timeouts)
- Worker integrations:
  - `GROQ_API_KEY`
  - `GOOGLE_API_KEY`, `GOOGLE_CSE_ID`
  - `PINECONE_API_KEY`, `PINECONE_INDEX_NAME`
  - `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD`
- Realtime/auth:
  - `REDIS_URL`
  - `ROOM_PASSWORD` and room credential settings

## Canonical Contract References

- `socket-hub/app/main.py`
- `dispatcher/app/main.py`
- `dispatcher/app/utils/schemas.py`
- `worker/app/main.py`

Last verified against code: February 13, 2026
