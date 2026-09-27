# Workflow: build an analysis contract

Before running any GIS operation, turn the natural-language question into an
explicit, structured plan. This gives reproducibility, auditability and a clear
place to check the CARE guardrail.

## Step 1 — decompose the question

```
QUESTION
├── geography          (where)
├── population         (who / what phenomenon)
├── condition          (environmental / temporal qualifier)
└── desired_result     (intersection, statistic, ranking, map, ...)
```

## Step 2 — write the contract

```yaml
analysis:
  question: >
    <the user's question, restated>
geography:
  country: <ISO 3166 or null>
  region: <e.g. Amazonia>
concepts:
  - role: population # or phenomenon / condition
    concept: <e.g. indigenous_population>
    indicator_id: <resolved via find_indicator + get_indicator>
    sensitive: <true|false>
operations:
  - <resolve_area>
  - <query_arcgis per indicator, filtered to AOI>
  - <spatial_intersection>
outputs:
  - map
  - table
  - chart
  - report
```

## Step 3 — check before executing

- Every `indicator_id` was verified with `get_indicator` + `get_layer_schema`.
- Any `sensitive: true` concept is routed to human review (see care-guardrail).
- Units are compatible for the operation you plan.
- The AOI polygon exists (from `resolve_area`).

Only then execute the operations in order, keeping the trace.
