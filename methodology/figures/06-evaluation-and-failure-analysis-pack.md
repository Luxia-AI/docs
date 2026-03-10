# evaluation and failure analysis pack

This pack defines publication-ready figure specs and Mermaid drafts.

### F35 — Metrics stack map (Acc/F1/ECE/Brier/NLL)

- **Figure ID**: F35
- **Paper Section**: Evaluation
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How do performance and calibration metrics relate?
- **Key Variables**: accuracy_3class, macro_ece, brier, nll, class_f1

#### Mermaid Block
```mermaid
flowchart TB
  M[Evaluation Metrics Stack] --> M1[Accuracy / Macro-F1]
  M --> M2[Calibration
ECE, Brier, NLL]
  M --> M3[Stability
conflict rate, flip rate]
  M --> M4[Retrieval Quality
KG/VDB balance, admission]
  M1 --> O[Version report]
  M2 --> O
  M3 --> O
  M4 --> O
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F35.* Metrics stack map (Acc/F1/ECE/Brier/NLL). This figure operationalizes how do performance and calibration metrics relate? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: evaluation/artifacts/metrics.json
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F36 — Version progression timeline

- **Figure ID**: F36
- **Paper Section**: Evaluation
- **Type**: curve
- **Placement**: Main
- **Column Fit**: 2-column
- **Research Question**: How do metrics evolve by version?
- **Key Variables**: version, accuracy, ece, p95

#### Mermaid Block
```mermaid
flowchart LR
  V1[v2 baseline] --> V2[v3.x iterations]
  V2 --> V3[v4/v5 policy changes]
  V3 --> V4[v6 contradiction fixes]
  V4 --> V5[v7-v8 deterministic owner]
  V5 --> M[Trend overlay
Accuracy, ECE, P95]
  M --> D[Regression checkpoints]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F36.* Version progression timeline. This figure operationalizes how do metrics evolve by version? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: analyze_runs by_version outputs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 2-column in final manuscript; keep text >= 8pt at print scale.

---
### F37 — Error taxonomy Sankey

- **Figure ID**: F37
- **Paper Section**: Failure Analysis
- **Type**: causal
- **Placement**: Main
- **Column Fit**: 2-column
- **Research Question**: How do upstream failures propagate into final errors?
- **Key Variables**: failure_category, downstream_outcome

#### Mermaid Block
```mermaid
flowchart LR
  E[All errors] --> E1[Retrieval miss]
  E --> E2[Alignment miss]
  E --> E3[Stance misclassification]
  E --> E4[Policy conflict]
  E1 --> O1[Wrong/abstain verdict]
  E2 --> O1
  E3 --> O1
  E4 --> O1
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F37.* Error taxonomy Sankey. This figure operationalizes how do upstream failures propagate into final errors? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: failure diagnostics + debug fields
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 2-column in final manuscript; keep text >= 8pt at print scale.

---
### F38 — Failure propagation chain diagram

- **Figure ID**: F38
- **Paper Section**: Failure Analysis
- **Type**: flowchart
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: What stage interactions amplify errors?
- **Key Variables**: stage_failure_flags, policy_path

#### Mermaid Block
```mermaid
flowchart TD
  F1[Parsing weakness] --> F2[Weak queries]
  F2 --> F3[Low-quality retrieval]
  F3 --> F4[Stance uncertainty]
  F4 --> F5[Low directional mass]
  F5 --> F6[UNVERIFIABLE collapse or wrong binary]
  F6 --> F7[Metric degradation]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F38.* Failure propagation chain diagram. This figure operationalizes what stage interactions amplify errors? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: stage_events + policy trace
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F39 — Alignment-zero cohort analysis

- **Figure ID**: F39
- **Paper Section**: Failure Analysis
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How do low-alignment cases differ from aligned cases?
- **Key Variables**: alignment_zero_rate, accuracy_in_cohort

#### Mermaid Block
```mermaid
flowchart LR
  C[Alignment=0 cohort] --> A1[Evidence topical but non-claim-specific]
  C --> A2[Predicate/object mismatch]
  C --> A3[Temporal/population mismatch]
  A1 --> R[High neutral rate]
  A2 --> R
  A3 --> R
  R --> O[Action: query/admission refinement]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F39.* Alignment-zero cohort analysis. This figure operationalizes how do low-alignment cases differ from aligned cases? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: new diagnostics in analyze_runs
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F40 — Confident-wrong decomposition

- **Figure ID**: F40
- **Paper Section**: Failure Analysis
- **Type**: curve
- **Placement**: Appendix
- **Column Fit**: 1-column
- **Research Question**: Which conditions produce confident wrong predictions?
- **Key Variables**: confident_wrong_count, confidence, target/pred mismatch

#### Mermaid Block
```mermaid
flowchart TD
  W[Confident Wrong Cases] --> W1[High confidence + false TRUE]
  W --> W2[High confidence + false FALSE]
  W1 --> D1[Support over-admission risk]
  W2 --> D2[Contradiction over-admission risk]
  D1 --> M[Mitigation
calibration + admission guards]
  D2 --> M
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F40.* Confident-wrong decomposition. This figure operationalizes which conditions produce confident wrong predictions? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: evaluation samples + confidence fields
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---
### F41 — KG/VDB imbalance dashboard layout

- **Figure ID**: F41
- **Paper Section**: Failure Analysis
- **Type**: table-graphic
- **Placement**: Main
- **Column Fit**: 1-column
- **Research Question**: How does retrieval-source imbalance track quality?
- **Key Variables**: kg_utilization_ratio, kg_zero_signal_rate, semantic_hits

#### Mermaid Block
```mermaid
flowchart LR
  K[KG contribution ratio] --> B[Balance monitor]
  V[VDB contribution ratio] --> B
  B --> Z1[KG-dominant neutral risk]
  B --> Z2[VDB-dominant semantic drift risk]
  Z1 --> A[Adaptive weighting]
  Z2 --> A
  A --> O[Balanced retrieval mix KPI]
```

#### Figure Spec (Camera-Ready)
- **Caption (IEEE/ACM style)**: *F41.* KG/VDB imbalance dashboard layout. This figure operationalizes how does retrieval-source imbalance track quality? using deterministic system signals and stage-linked diagnostics.
- **How to Read**: Start from the leftmost/topmost stage, follow directed transitions, then interpret terminal nodes against the metrics listed in the data-source field.
- **Expected Insight**: Reveals causal or procedural structure needed to reproduce and audit methodological behavior.
- **Failure Signal to Watch**: Disagreement between directional outputs and supporting upstream evidence signals; review `alignment_score`, `neutral_only_stance_rate`, and policy path branches.
- **Data Source / Log Fields**: metrics + debug vector_hits/kg_hits
- **Export Notes**: SVG/PDF export preferred; grayscale-safe palette required; annotate as 1-column in final manuscript; keep text >= 8pt at print scale.

---


