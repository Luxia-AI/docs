# Testing and Validation

## Goals

- Validate end-to-end claim processing behavior
- Validate contract consistency across Socket.IO, HTTP, and worker payloads
- Track regressions in latency and evidence quality

## Core Validation Paths

1. Service-level health checks
2. Socket integration flow
3. Worker verification API behavior
4. Metrics and observability checks

## Realtime Integration Script

`test_socketio_post.py` is used for interactive end-to-end checks and now includes:

- one-connection multi-claim execution
- run-scoped log output in `socketio_run_logs/`
- truth score input support per claim
- formatted metric/verdict output

Use this script to validate:

- auth and room flow
- dispatch and worker completion
- result payload quality and trust metrics
- per-run latency patterns

## Backend Verification Targets

- `dispatcher/app/main.py` dispatch submit path
- `worker/app/main.py` verify path and fallback behavior
- `socket-hub/app/main.py` event handling and emissions

## Recommended Validation Checklist

- Confirm all services reachable (`/`, `/healthz` where available)
- Submit at least one claim expected TRUE and one expected FALSE/PARTIALLY_TRUE case
- Confirm `worker_update` payload includes trust and evidence fields
- Confirm no contract key drift in payload structure
- Confirm metrics endpoint responds for each backend service

Last verified against code: February 13, 2026
