# Hybrid Corrective RAG Methodology

This directory is the implementation-grounded methodology reference for Luxia's hybrid corrective RAG fact verification pipeline.

## Scope

This methodology documents how the current system works in code, not how a generic RAG system should work.

Primary implementation sources:

- `socket-hub/app/main.py`
- `socket-hub/app/sockets/manager.py`
- `control-plane-api/app/routers/v1.py`
- `dispatcher/app/main.py`
- `worker/app/main.py`
- `worker/app/services/corrective/pipeline/__init__.py`
- `worker/app/services/retrieval/*`
- `worker/app/services/vdb/*`
- `worker/app/services/kg/*`
- `worker/app/services/ranking/*`
- `worker/app/services/verdict/*`
- `frontend/app/client/page.tsx`

## Reading Order

1. [01-system-architecture.md](./01-system-architecture.md)
2. [02-pipeline-stage-decomposition.md](./02-pipeline-stage-decomposition.md)
3. [03-retrieval-and-ingestion.md](./03-retrieval-and-ingestion.md)
4. [04-ranking-trust-and-corrective-loop.md](./04-ranking-trust-and-corrective-loop.md)
5. [05-verdict-synthesis.md](./05-verdict-synthesis.md)
6. [06-failure-modes-and-tradeoffs.md](./06-failure-modes-and-tradeoffs.md)
7. [07-system-dataflow-and-innovations.md](./07-system-dataflow-and-innovations.md)

## Section Schema Used Across Components

Each component section uses the same six-part schema:

1. Functional role in the system
2. Technical mechanism (internal operation)
3. Inputs and outputs
4. Interaction with other components
5. Why it is necessary in this hybrid pipeline
6. Failure points and design trade-offs

## Notation

- `VDB` means Pinecone vector storage and retrieval path.
- `KG` means Neo4j relation graph path.
- `Adaptive trust` means coverage/diversity/agreement-based sufficiency gating.
- `Corrective loop` means incremental search -> scrape -> extract -> ingest -> re-rank.

## Diagram Policy

Each major methodology file includes Mermaid diagrams plus prose equivalents. Diagrams are explanatory overlays; prose is complete and authoritative on its own.

## Boundary Conditions

- No runtime APIs are changed by this documentation set.
- Existing docs under `docs/` are treated as non-authoritative unless revalidated against code.
- Terminology baseline: `hybrid corrective RAG fact verification pipeline`.

Last verified against code: February 28, 2026
