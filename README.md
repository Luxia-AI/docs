# Luxia Documentation Hub

This documentation set is now code-first and aligned to the current implementation.

If you are writing or reviewing methodology sections, start here:

- [Methodology Index](./methodology/README.md)

## Fast Start

1. [system-overview.md](./system-overview.md)
2. [request-lifecycle.md](./request-lifecycle.md)
3. [worker-pipeline.md](./worker-pipeline.md)
4. [methodology/README.md](./methodology/README.md)

## Current Architecture Summary

- `socket-hub`: Socket.IO ingress, room auth, realtime fanout
- `control-plane-api`: token-scoped room/action authorization
- `dispatcher`: worker orchestration and delivery fallback logic
- `worker`: hybrid corrective RAG verification pipeline
- `frontend`: realtime stage/result consumer

Primary storage and retrieval systems:

- Pinecone (vector retrieval)
- Neo4j (graph retrieval)
- SQLite FTS5 (lexical BM25 side-channel)
- Redis (room queue and credentials)
- Kafka (optional async transport)

## Methodology Pack

The full implementation-level methodology is split for research-grade readability:

- [01-system-architecture.md](./methodology/01-system-architecture.md)
- [02-pipeline-stage-decomposition.md](./methodology/02-pipeline-stage-decomposition.md)
- [03-retrieval-and-ingestion.md](./methodology/03-retrieval-and-ingestion.md)
- [04-ranking-trust-and-corrective-loop.md](./methodology/04-ranking-trust-and-corrective-loop.md)
- [05-verdict-synthesis.md](./methodology/05-verdict-synthesis.md)
- [06-failure-modes-and-tradeoffs.md](./methodology/06-failure-modes-and-tradeoffs.md)
- [07-system-dataflow-and-innovations.md](./methodology/07-system-dataflow-and-innovations.md)

## Notes

- Existing docs are treated as implementation references only when validated against source code.
- No runtime contracts are changed by this docs refresh.

Last verified against code: February 28, 2026
