# Deployment and Operations

## Local Compose Topology

Primary runtime is defined in `docker-compose.yml` and includes:

- core services: `socket-hub`, `dispatcher`, `worker`
- messaging/state: `kafka`, `zookeeper`, `redis`
- observability: `prometheus`, `alertmanager`, `loki`, `promtail`, `tempo`, `otel-collector`, `grafana`

## Azure Unified Container Mode

`infra/azure` packages all backend services into one container:

- Nginx ingress on port 80
- Supervisord manages:
  - `socket-hub` (uvicorn on 127.0.0.1:8000)
  - `dispatcher` (uvicorn on 127.0.0.1:8001)
  - `worker` (uvicorn on 127.0.0.1:8002)
- Nginx route prefixes:
  - `/` -> socket-hub
  - `/dispatcher/` -> dispatcher
  - `/worker/` -> worker
  - `/metrics/{service}` -> service metrics

## Observability

Shared metrics middleware is installed by all services through `shared/metrics.py`.

Backend metrics endpoints:

- socket-hub: `GET /metrics`
- dispatcher: `GET /metrics`
- worker: `GET /metrics`

Tracing is configured for OTLP export; stack includes Tempo and OTel Collector in compose.

## Runtime Controls

- High timeout defaults are configured for long-running verification workloads.
- Kafka reliability knobs include retries/backoff/request timeout.
- Worker preload behavior is controlled by `WORKER_PRELOAD_PIPELINE`.

## Infra Status Notes

- `infra/k8s/backend-deployment.yml`: currently empty
- `infra/k8s/frontend-deployment.yml`: currently empty
- `infra/terraform/main.tf`: currently empty

These are placeholders and not authoritative deployment sources at this time.

Last verified against code: February 13, 2026
