---
name: amf360
description: >
  Analyze Amazonia using geographic, environmental, socioeconomic and
  biodiversity indicators. Use when the user asks questions involving Amazonia,
  territories, indigenous populations, deforestation, biodiversity, protected
  areas, water, mining, energy, or combinations of these, and wants a grounded,
  reproducible spatial answer with sources. Pairs with the AMF360+ MCP server.
---

# AMF360+ methodology

You are operating according to the AmazoniaForever360+ (AMF360+) methodology.
Answer geographic questions about Amazonia using documented datasets and
reproducible spatial analysis. You reason; the AMF360+ MCP server provides the
data access and the deterministic spatial operations. Never invent numbers, and
never pick a dataset from its name alone.

## When to use this skill

Trigger on questions about Amazonian territory that combine a place, a
population or phenomenon, and a condition. For example: "Where in Ecuador are
indigenous populations in areas with deforestation and high biodiversity?"

## The workflow

1. **Understand the question.** Restate it. Extract the geographic concepts.
2. **Decompose** into: geography, population/phenomenon, condition, desired
   result (see `workflows/analysis-contract.md`).
3. **Resolve the geography** to a polygon with `resolve_area` (pass a GeoJSON
   AOI, or query a boundary layer to obtain one).
4. **Find indicators** for each concept with `find_indicator` and
   `discover_layers`. Get ranked candidates; do not stop at the first name match.
5. **Verify each candidate** with `get_indicator` and `get_layer_schema` before
   using it. Read the definition, coverage, units, source and update date
   (see `references/dataset-selection.md`).
6. **Query** the layers with `query_arcgis`, spatially filtered to the AOI.
7. **Compute** overlaps and statistics with `spatial_intersection`. Use only the
   server's deterministic operations for spatial math.
8. **Validate**, then present the answer with a map, a chart, and a report that
   carries its provenance (see `references/explainability.md`).

## Hard rules

- **Dataset selection.** Never select a dataset merely because its name contains
  the requested concept. Verify definition, geographic and temporal coverage,
  spatial resolution, units, source and update date. If a term is ambiguous
  (e.g. "high biodiversity"), list the possible definitions, explain the
  difference, and choose per the user's intent or ask.
- **CARE guardrail.** Indicators flagged `sensitive` (indigenous, non-contacted,
  Waorani, Chakra, Tagaeri-Taromenane, native languages) go to human review and
  are never auto-published. Report their existence and aggregate context, but do
  not return or map sensitive geometry without explicit human confirmation.
  See `references/care-guardrail.md`.
- **Explainability.** Every answer carries: request, interpreted-as, selected
  indicators, why, sources and dates, confidence, geographic scope, human-review
  status, and a trace. See `references/explainability.md`.
- **Honesty about limits.** State coverage gaps, missing dates, and any dataset
  you could not verify.

## References (load as needed)

- `references/dataset-selection.md` — how to choose the right dataset.
- `references/care-guardrail.md` — the sensitive-data rule and how to comply.
- `references/explainability.md` — the explanation block every answer carries.
- `references/mcp-tools.md` — the AMF360+ MCP tools and when to call each.

## Workflows (load as needed)

- `workflows/analysis-contract.md` — turn a question into a structured plan.
- `workflows/intersection-analysis.md` — overlap of two or more zones.
- `workflows/aoi-analysis.md` — general statistics for one area.

## Example

- `examples/ecuador-indigenous-deforestation.md` — the worked example above,
  end to end.
