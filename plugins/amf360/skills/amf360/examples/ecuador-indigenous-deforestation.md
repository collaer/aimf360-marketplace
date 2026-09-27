# Worked example: indigenous + deforestation + biodiversity in Ecuador

**Question (as asked):**
"/amf360 ¿Dónde en Ecuador hay poblaciones indígenas en zona en deforestación y
con alta biodiversidad?"

This example shows the full method, including how the CARE guardrail changes the
answer.

## 1. Decompose (analysis contract)

```yaml
geography: { country: EC, region: Amazonia }
concepts:
  - role: population   concept: indigenous_population   sensitive: true
  - role: phenomenon   concept: deforestation           sensitive: false
  - role: condition    concept: high_biodiversity       sensitive: false
desired_result: intersection (where all three coincide)
```

## 2. Resolve the geography

The AOI is the Ecuadorian Amazon. Obtain it by querying the Amazonia boundary
layer intersected with the Ecuador national boundary, then pass the resulting
polygon to `resolve_area` as `geojson`.

## 3. Find and verify indicators

- Indigenous population → `find_indicator("indigenous territories")`. The
  candidate is flagged **sensitive**. It goes to human review; its geometry is
  not mapped without approval. Aggregate context only.
- Deforestation → `find_indicator("deforestation")`. In Ecuador prefer the
  national "Deforestacion 2020 2022" layer over the regional layer. Verify years
  and units with `get_layer_schema`.
- High biodiversity → this is ambiguous. List the options the data supports
  (ecosystems, biogeographic units, biosphere reserves, connectivity corridors)
  and choose the one matching the user's intent, or ask.

## 4. Execute (respecting CARE)

- Deforestation ∩ biodiversity: `query_arcgis` each, filtered to the AOI, then
  `spatial_intersection`. Report km2 and coverage.
- Indigenous overlap: because the layer is sensitive, do **not** map it. Report
  only aggregate context (for example: whether indigenous territories are present
  within the deforestation-and-biodiversity zone, as a count), and mark
  `human_review: required` in the answer.

## 5. Answer with the explanation block

The answer states: the AOI (Ecuadorian Amazon), the deforestation and
biodiversity layers used with their dates, the intersection result, and clearly
that the indigenous overlap needs human review and was not published as geometry.
Confidence is set from dataset freshness and the ambiguity of "high biodiversity".

## Why this is the point of the platform

A naive system would map the indigenous layer directly. The AMF360+ method
produces a grounded, sourced answer and protects sensitive data by default —
correctness and ethics over a plausible-looking map.
