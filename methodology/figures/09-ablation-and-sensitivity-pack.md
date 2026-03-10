# ablation and sensitivity pack

This pack defines publication-ready figure specs and Mermaid drafts.

### F50 — Retrieval weight ablation matrix

- **Figure ID**: F50
- **Paper Section**: Ablations
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How do retrieval weights alter accuracy and calibration?
- **Key Variables**: w_vdb,w_kg,accuracy,ece

#### Mermaid Block
```mermaid
flowchart TB
  W[Retrieval weight settings] --> W1[High VDB / Low KG]
  W --> W2[Balanced]
  W --> W3[Low VDB / High KG]
  W1 --> M[Measure Acc/F1/ECE/conflict]
  W2 --> M
  W3 --> M
  M --> S[Select stable weighting region]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F50.* Retrieval weight ablation matrix. This figure operationalizes how do retrieval weights alter accuracy and calibration? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: ablation experiments
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F51 — Trust-threshold sensitivity curves

- **Figure ID**: F51
- **Paper Section**: Ablations
- **Type**: curve
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How sensitive are outcomes to trust threshold choices?
- **Key Variables**: trust_threshold, accuracy,ece,abstain_rate

#### Mermaid Block
```mermaid
flowchart LR
  T1[Low trust threshold] --> R1[High recall, low precision]
  T2[Medium threshold] --> R2[Balanced precision/recall]
  T3[High threshold] --> R3[Low recall, high precision]
  R1 --> O[Operating curve]
  R2 --> O
  R3 --> O
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F51.* Trust-threshold sensitivity curves. This figure operationalizes how sensitive are outcomes to trust threshold choices? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: threshold sweep runs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F52 — Calibration method comparison panel

- **Figure ID**: F52
- **Paper Section**: Ablations
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: Which calibration method best improves ECE without hurting accuracy?
- **Key Variables**: method,ece,brier,nll,acc

#### Mermaid Block
```mermaid
flowchart TB
  C[Calibration methods] --> C1[Temperature scaling]
  C --> C2[Isotonic regression]
  C --> C3[No calibration baseline]
  C1 --> E[Compare ECE/Brier/NLL]
  C2 --> E
  C3 --> E
  E --> S[Method selection for deployment]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F52.* Calibration method comparison panel. This figure operationalizes which calibration method best improves ece without hurting accuracy? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: calibration experiment logs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F53 — Query-strategy ablation map

- **Figure ID**: F53
- **Paper Section**: Ablations
- **Type**: flowchart
- **Placement**: Appendix
- **Column Fit**: 1-column
- **Research Question**: How do query strategy variants impact corrective effectiveness?
- **Key Variables**: strategy,queries_used,alignment_zero_rate,acc

#### Mermaid Block
```mermaid
flowchart LR
  Q[Query strategy variants] --> Q1[support-heavy]
  Q --> Q2[balanced support/refute]
  Q --> Q3[authority-heavy]
  Q1 --> M[Measure contradiction admission
+ final metrics]
  Q2 --> M
  Q3 --> M
  M --> O[Recommended strategy map]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F53.* Query-strategy ablation map. This figure operationalizes how do query strategy variants impact corrective effectiveness? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: query strategy experiment runs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---


