# retrieval and corrective pack

This pack defines publication-ready figure specs and Mermaid drafts.

### F11 — Hybrid retrieval orchestration sequence

- **Figure ID**: F11
- **Paper Section**: Methodology: Retrieval
- **Type**: sequence
- **Placement**: Main
- **Column Fit**: 2-column
- **Research Question**: How do VDB and KG retrieval interact with corrective stages?
- **Key Variables**: queries, semantic_candidates_count, kg_candidates_count

#### Mermaid Block
```mermaid
sequenceDiagram
  participant W as Worker
  participant V as VectorDB
  participant K as KG
  participant S as Web Search
  participant R as Ranker
  W->>V: semantic_query(claim_frame)
  W->>K: graph_query(entities,predicate)
  V-->>W: vector_candidates
  K-->>W: kg_candidates
  W->>R: merge_and_rank(candidates)
  alt insufficient evidence
    W->>S: corrective_query(batch)
    S-->>W: corrective_candidates
    W->>R: rerank_with_new_candidates
  end
  R-->>W: ranked_evidence_pool
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F11.* Hybrid retrieval orchestration sequence. This figure operationalizes how do vdb and kg retrieval interact with corrective stages? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: retrieval phase outputs; debug.generated_queries
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 2-column in final manuscript; keep text >= 8pt at print scale.

---
### F12 — VDB retrieval decision surface

- **Figure ID**: F12
- **Paper Section**: Methodology: Retrieval
- **Type**: curve
- **Placement**: Appendix
- **Column Fit**: 1-column
- **Research Question**: How does semantic thresholding influence recall/noise?
- **Key Variables**: vdb_score, min_threshold, dropped_claim_mismatch

#### Mermaid Block
```mermaid
flowchart TB
  Q[Query Embedding] --> S1[Similarity Score]
  S1 --> F1[Filter by semantic_floor]
  F1 --> K1[Top-k selection]
  K1 --> D1[Directness scoring]
  D1 --> O1[Pass to fusion]
  subgraph Decision Surface
    A1[high similarity + high directness -> admit]
    A2[high similarity + low directness -> neutral risk]
    A3[low similarity + high directness -> keep for review]
    A4[low similarity + low directness -> drop]
  end
  O1 -. classify .-> Decision Surface
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F12.* VDB retrieval decision surface. This figure operationalizes how does semantic thresholding influence recall/noise? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: VDB retrieval filter logs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F13 — KG traversal and expansion graph

- **Figure ID**: F13
- **Paper Section**: Methodology: Retrieval
- **Type**: DAG
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How are entities expanded into KG relations?
- **Key Variables**: seed_entities, hop, kg_score, kg_fallback_triggered

#### Mermaid Block
```mermaid
flowchart LR
  E[Claim Entities] --> N1[Seed KG Nodes]
  N1 --> X1[Predicate-constrained Expansion]
  X1 --> X2[Neighbor Pruning<br/>confidence + relation type]
  X2 --> P1[Path Scoring]
  P1 --> C1[Candidate Assertions]
  C1 --> F1[Retrieval Fusion Input]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F13.* KG traversal and expansion graph. This figure operationalizes how are entities expanded into kg relations? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: KG retrieval outputs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F14 — Retrieval fusion DAG

- **Figure ID**: F14
- **Paper Section**: Methodology: Retrieval
- **Type**: DAG
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How are VDB and KG candidates merged before ranking?
- **Key Variables**: semantic_score, kg_score, dedup_count

#### Mermaid Block
```mermaid
flowchart TD
  V[Vector Candidates] --> U[Unified Candidate Pool]
  K[KG Candidates] --> U
  W[Web Corrective Candidates] --> U
  U --> R1[Normalize Scores]
  R1 --> R2[Source-aware Reweighting]
  R2 --> R3[Deduplicate by canonical source+span]
  R3 --> R4[Top-N ranked evidence]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F14.* Retrieval fusion DAG. This figure operationalizes how are vdb and kg candidates merged before ranking? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: retrieval_phase semantic_dedup_count + kg_with_score
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F15 — Query generation lattice (support/refute tracks)

- **Figure ID**: F15
- **Paper Section**: Methodology: Corrective Retrieval
- **Type**: flowchart
- **Placement**: Main
- **Column Fit**: 2-column
- **Research Question**: How are support-track and refute-track queries generated?
- **Key Variables**: queries_planned, query_track, query_quality

#### Mermaid Block
```mermaid
flowchart LR
  C[Canonical Claim Frame] --> Q1[Support Track Queries]
  C --> Q2[Refute Track Queries]
  C --> Q3[Authority Track Queries]
  C --> Q4[Exact/Rephrase Track]
  Q1 --> D[Query Dedup + Quality Filter]
  Q2 --> D
  Q3 --> D
  Q4 --> D
  D --> E[Executable Query Lattice<br/>with facet tags]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F15.* Query generation lattice (support/refute tracks). This figure operationalizes how are support-track and refute-track queries generated? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: debug.generated_queries + corrective loop logs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 2-column in final manuscript; keep text >= 8pt at print scale.

---
### F16 — Corrective loop state machine

- **Figure ID**: F16
- **Paper Section**: Methodology: Corrective Retrieval
- **Type**: state
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: What states and exits govern iterative corrective retrieval?
- **Key Variables**: query_budget_total, query_budget_used, stop_reason

#### Mermaid Block
```mermaid
stateDiagram-v2
  [*] --> InitialRetrieve
  InitialRetrieve --> Evaluate
  Evaluate --> Stop: sufficient evidence
  Evaluate --> CorrectiveSearch: insufficiency
  CorrectiveSearch --> Extract
  Extract --> Evaluate: yield > 0
  Extract --> Penalize: zero_yield
  Penalize --> Evaluate
  Penalize --> Stop: budget exhausted
  Stop --> [*]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F16.* Corrective loop state machine. This figure operationalizes what states and exits govern iterative corrective retrieval? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: debug.query_budget + debug.stop_reason
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F17 — Query budget and stop-criterion flow

- **Figure ID**: F17
- **Paper Section**: Methodology: Corrective Retrieval
- **Type**: flowchart
- **Placement**: Appendix
- **Column Fit**: 1-column
- **Research Question**: When does the loop continue vs stop under confidence mode?
- **Key Variables**: coverage, diversity, adaptive_sufficient, gain

#### Mermaid Block
```mermaid
flowchart TD
  S[Start Round] --> B1{query_budget_used < total?}
  B1 -- no --> END[Stop: budget exhausted]
  B1 -- yes --> R[Run Next Query Batch]
  R --> Y{new directional evidence?}
  Y -- yes --> C[Increase sufficiency]
  Y -- no --> P[Decrease sufficiency<br/>zero-yield penalty]
  C --> G{stop criterion met?}
  P --> G
  G -- yes --> END
  G -- no --> S
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F17.* Query budget and stop-criterion flow. This figure operationalizes when does the loop continue vs stop under confidence mode? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: pipeline stop decision logs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F18 — Source acquisition funnel

- **Figure ID**: F18
- **Paper Section**: Methodology: Retrieval
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How do fetched URLs progress to usable evidence?
- **Key Variables**: urls_requested, pages_scraped, facts_count, admitted_evidence

#### Mermaid Block
```mermaid
flowchart TB
  A[All retrieved URLs] --> B[Trusted-domain filter]
  B --> C[Content-type filter]
  C --> D[Extractable page filter]
  D --> E[Claim-directness filter]
  E --> F[Admissible evidence set]
  A -. rejected .-> R1[Rejected: off-domain]
  C -. rejected .-> R2[Rejected: unsupported mime]
  D -. rejected .-> R3[Rejected: extraction failure]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F18.* Source acquisition funnel. This figure operationalizes how do fetched urls progress to usable evidence? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: scraper/fact_extractor phase outputs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---


