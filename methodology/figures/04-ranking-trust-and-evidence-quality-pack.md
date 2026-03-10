# ranking trust and evidence quality pack

This pack defines publication-ready figure specs and Mermaid drafts.

### F19 — Candidate ranking pipeline

- **Figure ID**: F19
- **Paper Section**: Methodology: Ranking
- **Type**: flowchart
- **Placement**: Main
- **Column Fit**: 2-column
- **Research Question**: How are candidates transformed into top-k evidence?
- **Key Variables**: final_score, sem_score, kg_score, credibility

#### Mermaid Block
```mermaid
flowchart LR
  I[Candidate Evidence] --> F1[Feature Build<br/>retrieval/alignment/stance/trust]
  F1 --> S1[Score Normalization]
  S1 --> S2[Hybrid Rank Score]
  S2 --> G1[Guardrails<br/>source quality + diversity]
  G1 --> T[Top-K shortlist]
  T --> O[Verdict Aggregation Input]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F19.* Candidate ranking pipeline. This figure operationalizes how are candidates transformed into top-k evidence? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: ranking_phase top_k_selected
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 2-column in final manuscript; keep text >= 8pt at print scale.

---
### F20 — Stance-aware reranking matrix

- **Figure ID**: F20
- **Paper Section**: Methodology: Ranking
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How do support/refute/neutral stances interact with rank?
- **Key Variables**: support_score, contradict_score, stance

#### Mermaid Block
```mermaid
flowchart TB
  subgraph MatrixAxes[Stance-Aware Matrix]
    X1[Rows: support/refute/neutral logits]
    X2[Cols: predicate alignment, object match, source trust]
  end
  MatrixAxes --> C1[Compute stance-aware rank delta]
  C1 --> C2[Boost contradiction when verified]
  C2 --> C3[Penalize neutral-only dominance]
  C3 --> O[Re-ranked list]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F20.* Stance-aware reranking matrix. This figure operationalizes how do support/refute/neutral stances interact with rank? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: ranking scores + evidence_map relevance
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F21 — Trust score decomposition

- **Figure ID**: F21
- **Paper Section**: Methodology: Trust Policy
- **Type**: DAG
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How is trust_post composed from coverage/diversity/agreement?
- **Key Variables**: coverage, diversity, agreement, trust_post

#### Mermaid Block
```mermaid
flowchart LR
  T[Trust Score] --> C[Source Credibility]
  T --> Q[Evidence Quality]
  T --> A[Alignment Strength]
  T --> D[Diversity Contribution]
  C --> W[Weighted Trust Composite]
  Q --> W
  A --> W
  D --> W
  W --> O[Admissibility/Priority Signal]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F21.* Trust score decomposition. This figure operationalizes how is trust_post composed from coverage/diversity/agreement? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: adaptive_trust_policy logs + payload.trust_post
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F22 — Evidence admissibility gate

- **Figure ID**: F22
- **Paper Section**: Methodology: Evidence Validation
- **Type**: flowchart
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: Which gates decide admissible vs rejected evidence?
- **Key Variables**: anchor_match, predicate_match, object_match_ok, domain_credibility

#### Mermaid Block
```mermaid
flowchart TD
  E[Evidence Item] --> G1{source trusted?}
  G1 -- no --> R1[Reject: low credibility]
  G1 -- yes --> G2{claim alignment >= threshold?}
  G2 -- no --> R2[Reject: weak alignment]
  G2 -- yes --> G3{stance confidence >= threshold?}
  G3 -- no --> R3[Keep as neutral]
  G3 -- yes --> A[Admit directional evidence]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F22.* Evidence admissibility gate. This figure operationalizes which gates decide admissible vs rejected evidence? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: evidence_map alignment/validation fields
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F23 — Alignment scoring anatomy

- **Figure ID**: F23
- **Paper Section**: Methodology: Evidence Validation
- **Type**: causal
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: Which factors drive alignment score behavior?
- **Key Variables**: alignment_score, anchor_overlap, predicate_match_score

#### Mermaid Block
```mermaid
flowchart LR
  C[Claim Frame] --> SA[Subject Alignment]
  C --> PA[Predicate Alignment]
  C --> OA[Object Alignment]
  C --> QA[Qualifier Alignment]
  SA --> F[Alignment Fusion]
  PA --> F
  OA --> F
  QA --> F
  F --> S[Final alignment_score]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F23.* Alignment scoring anatomy. This figure operationalizes which factors drive alignment score behavior? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: debug.alignment_score + evidence-level features
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F24 — Contradiction admission path

- **Figure ID**: F24
- **Paper Section**: Methodology: Ranking
- **Type**: flowchart
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How are contradiction candidates admitted and verified?
- **Key Variables**: refute_candidate_count_stage1, refute_verified_count_stage2

#### Mermaid Block
```mermaid
flowchart TD
  E[Candidate Evidence] --> C1[Compute contradiction_score]
  C1 --> C2[Evaluate refute_eligibility_score]
  C2 --> G{eligibility >= threshold?}
  G -- no --> N[Not admitted to refute<br/>block_reason logged]
  G -- yes --> V{anti-hallucination checks pass?}
  V -- no --> N2[Rejected by guardrail]
  V -- yes --> R[Admit to contradiction mass]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F24.* Contradiction admission path. This figure operationalizes how are contradiction candidates admitted and verified? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: refute_pipeline_stats
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F25 — Evidence diversity vs agreement map

- **Figure ID**: F25
- **Paper Section**: Methodology: Trust Policy
- **Type**: curve
- **Placement**: Appendix
- **Column Fit**: 1-column
- **Research Question**: What trade-off exists between diversity and agreement?
- **Key Variables**: diversity, agreement, trust_threshold_met

#### Mermaid Block
```mermaid
flowchart LR
  A[Low diversity + high agreement] --> Z1[Overfit risk]
  B[High diversity + high agreement] --> Z2[Strong robust signal]
  C[High diversity + low agreement] --> Z3[Uncertain / mixed]
  D[Low diversity + low agreement] --> Z4[Insufficient evidence]
  Z1 --> P[Confidence penalty]
  Z3 --> P
  Z4 --> P
  Z2 --> Q[Confidence uplift]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F25.* Evidence diversity vs agreement map. This figure operationalizes what trade-off exists between diversity and agreement? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: trust_snapshot/debug.trust_gate
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F26 — Neutral-only evidence detection flow

- **Figure ID**: F26
- **Paper Section**: Methodology: Evidence Validation
- **Type**: flowchart
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How are neutral-only evidence regimes flagged?
- **Key Variables**: supports_count, refutes_count, neutral_count

#### Mermaid Block
```mermaid
flowchart TD
  I[Ranked Evidence Set] --> C1[Count support/refute labels]
  C1 --> D{support=0 and refute=0?}
  D -- no --> N[Proceed directional policy]
  D -- yes --> C2[Check neutral evidence quality]
  C2 --> C3[Assign internal UNVERIFIABLE]
  C3 --> C4[Apply binary projection rule]
  C4 --> O[Output with abstain_reason]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F26.* Neutral-only evidence detection flow. This figure operationalizes how are neutral-only evidence regimes flagged? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: debug.evidence_stance_distribution
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---


