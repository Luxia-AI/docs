# Known Issues and Next Steps

## Known Issues

1. Documentation drift existed previously across module-local Markdown files.
2. Some infra targets are placeholders (`infra/k8s`, `infra/terraform` files are empty).
3. Worker evidence quality can vary by claim type and cached evidence mix.
4. Cross-service contracts are implemented but not yet versioned as formal schemas.

## Risks

- Confidence and verdict calibration risk for nuanced or comparative claims
- Retrieval noise in top evidence can affect rationale quality
- Operational complexity when toggling between HTTP and Kafka paths

## Priority Improvements

1. Add machine-validated API/Socket/Kafka schemas and CI contract checks.
2. Add benchmark claim suite with expected truth bands and automated regression scoring.
3. Harden trust gating acceptance criteria for low-diversity evidence sets.
4. Fill k8s/terraform deployment definitions or remove placeholders.
5. Add docs CI link checker and stale-doc detection.

## Recommended Near-Term Roadmap

1. Contract formalization
2. Retrieval relevance evaluation harness
3. Latency SLO and alerting thresholds per path
4. Deployment profile separation (dev, staging, prod)

Last verified against code: February 13, 2026
