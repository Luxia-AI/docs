# Figure Style Guide (IEEE/ACM, Dual Mermaid+Spec)

## Objective
This guide standardizes methodology figures for camera-ready submission while keeping them executable in Markdown via Mermaid.

## Notation
- IDs: `F01` to `F56`.
- Terminology: use deterministic labels (`support_mass`, `contradict_mass`, `alignment_score`, `verdict_binary`, `verdict_internal`).
- Arrows: causal or data/control flow only; avoid decorative arrows.

## Visual Defaults
- Palette (grayscale-safe):
  - Primary process: `#1F2937`
  - Evidence/retrieval: `#0B5FFF`
  - Trust/calibration: `#0E7C66`
  - Failure/risk: `#B54708`
  - Neutral background: `#F8FAFC`
- Use color + shape redundancy for accessibility.
- Typography target for exported figure: >=8pt body, >=9pt axis/labels.

## Mermaid Conventions
- Prefer `flowchart`, `graph`, `stateDiagram-v2`, `sequenceDiagram`, and `xychart-beta`.
- Keep node text concise and deterministic.
- One primary message per figure.

## Required Per-Figure Contract
Every figure section must include:
1. Figure ID
2. Paper Section
3. Type
4. Research Question
5. Key Variables
6. Mermaid Block
7. Caption (camera-ready style)
8. How to Read
9. Expected Insight
10. Failure Signal to Watch
11. Data Source / Log Fields
12. Export Notes

## Publication Defaults
- Tone: IEEE/ACM technical prose.
- Always annotate `1-column` or `2-column` fit.
- Export intent: SVG first, PDF second, PNG fallback.
