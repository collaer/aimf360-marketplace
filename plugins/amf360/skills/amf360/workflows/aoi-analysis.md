# Workflow: single-area (AOI) analysis

Produce general context for one area: total area, and the presence and extent of
selected phenomena within it.

## Steps

1. **Resolve the AOI** with `resolve_area` (GeoJSON polygon). Record area, bbox,
   centroid.
2. **Pick the descriptive indicators** the question needs (for example protected
   areas, deforestation, rivers, administrative units). Verify each with
   `get_indicator` + `get_layer_schema`.
3. **Query each** with `query_arcgis`, filtered to the AOI bbox.
4. **Summarize** per indicator: count of features, and where geometry allows,
   `spatial_intersection` for area and coverage share.
5. **Report** a compact table (indicator, value, unit, source, date) plus the
   explanation block.

## Good default descriptors for an Amazonian AOI

- Total area (km2) — from `resolve_area`.
- Protected / conservation areas — coverage share.
- Deforestation — extent within the AOI, with the year(s).
- Hydrography — main rivers / basins present.
- Administrative units — which level-1/2/3 units the AOI touches.

Keep sensitive layers (indigenous territories) to aggregate context only, under
the CARE guardrail.
