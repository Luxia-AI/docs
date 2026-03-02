# Deployment and Operations

This document captures the current runtime topology and operational controls for Luxia.

## Deployment Profiles

### 1) Local multi-container runtime (`docker-compose.yml`)

Core application services:

- `socket-hub`
- `dispatcher`
- `worker`
- `control-plane-api`

State and transport services:

- `redis`
- `kafka`
- `zookeeper`
- `control-plane-db` (PostgreSQL container)

Observability stack:

- `prometheus`
- `alertmanager`
- `loki`
- `promtail`
- `tempo`
- `otel-collector`
- `grafana`

### 2) Azure unified-container runtime (`infra/azure`)

Entry and process model:

- bootstrap script: `infra/azure/start.sh`
- process supervisor: `infra/azure/supervisord/supervisord.conf`
- reverse proxy: `infra/azure/nginx/nginx.conf`

Internal process ports (127.0.0.1):

- socket-hub: `8000`
- dispatcher: `8001`
- worker: `8002`
- control-plane-api: `8003`

Nginx routing:

- `/` -> `socket-hub`
- `/dispatcher/` -> `dispatcher`
- `/worker/` -> `worker`
- `/v1/` -> `control-plane-api`
- `/metrics/socket-hub` -> socket metrics
- `/metrics/dispatcher` -> dispatcher metrics
- `/metrics/worker` -> worker metrics

## Runtime Characteristics

- Backend services are FastAPI applications.
- `socket-hub` runs as Socket.IO ASGI app.
- Service metrics are exposed via `shared/metrics.py` middleware on `/metrics`.
- Worker runtime supports startup preload (`WORKER_PRELOAD_PIPELINE`) and fallback-completed behavior.
- Kafka path is optional and controlled by service-level environment toggles.

## Operational Controls

### Timeout controls

- Socket-hub dispatch envelope: `DISPATCH_CONNECT_TIMEOUT_SECONDS`, `DISPATCH_READ_TIMEOUT_SECONDS`, `DISPATCH_WRITE_TIMEOUT_SECONDS`, `DISPATCH_POOL_TIMEOUT_SECONDS`
- Dispatcher->worker envelope: `WORKER_CONNECT_TIMEOUT_SECONDS`, `WORKER_READ_TIMEOUT_SECONDS`, `WORKER_WRITE_TIMEOUT_SECONDS`, `WORKER_POOL_TIMEOUT_SECONDS`

### Transport mode controls

- Socket-hub: `SOCKETHUB_USE_REDIS`, `SOCKETHUB_USE_KAFKA`, `SOCKETHUB_USE_KAFKA_RESULTS`
- Dispatcher: `ENABLE_KAFKA`
- Callback hardening: `SOCKETHUB_RESULT_CALLBACK_TOKEN`

### Verification behavior controls

- Confidence mode: `LUXIA_CONFIDENCE_MODE`
- LLM budget: `GROQ_MAX_CALLS_PER_JOB`, related reserve settings
- Verdict v2 toggles: `VERDICT_ENGINE_V2_ENABLED`, `VERDICT_ENGINE_V2_SHADOW`, `VERDICT_ENGINE_V2_FAIL_OPEN`

## Observability and Telemetry

Data planes:

- Metrics: Prometheus scrape endpoints and service counters/histograms
- Logs: Loki + Promtail
- Traces: OTel collector -> Tempo
- Dashboards: Grafana provisioning under `infra/observability/grafana`

Important service-level metrics families:

- socket-hub connection/auth/dispatch counters and latency
- dispatcher dispatch counters and worker roundtrip latency
- worker completion/failure counters and pipeline diagnostics fields in payload

## Operational Risks and Notes

1. Multi-path result delivery
- HTTP callback and Kafka result paths coexist; this improves resilience but complicates failure forensics.

2. Timeout sensitivity
- Corrective verification can be long-running; under-provisioned timeouts appear as false `error` states.

3. Infra placeholders
- `infra/k8s/backend-deployment.yml`
- `infra/k8s/frontend-deployment.yml`
- `infra/terraform/main.tf`

These files currently remain placeholders and are not authoritative deployment definitions.

## Canonical Runtime Files

- `docker-compose.yml`
- `infra/azure/start.sh`
- `infra/azure/supervisord/supervisord.conf`
- `infra/azure/nginx/nginx.conf`
- `infra/observability/**`

Last verified against code: March 2, 2026
