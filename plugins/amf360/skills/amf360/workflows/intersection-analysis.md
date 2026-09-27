# Workflow: intersection analysis

Answer "how much of X overlaps Y" or "where do A, B and C coincide".

## Steps

1. **Resolve the AOI.** `resolve_area` with a GeoJSON polygon, or query a
   boundary layer to obtain one, then pass it back as geojson.
2. **Get each layer's polygon.** For each concept, `query_arcgis` on its
   indicator, filtered to the AOI bbox, `returnGeometry=true`. Respect the CARE
   guardrail for sensitive layers.
3. **Intersect pairwise.** `spatial_intersection(aoi, layerA)` gives area and
   coverage. To combine three conditions, intersect A with B, then the result
   with C. Feed each intersection geometry forward as the next `aoi`.
4. **Quantify.** Report intersection km2 and `coveragePct` at each step.
5. **Explain.** Note the CRS (WGS84), the record caps hit, and confidence.

## Combining three conditions (A ∩ B ∩ C)

```
r1 = spatial_intersection(aoi=AOI, other=A)      -> geometry g1
r2 = spatial_intersection(aoi=g1,  other=B)      -> geometry g2
r3 = spatial_intersection(aoi=g2,  other=C)      -> final overlap
```

Set `includeGeometry=true` on the intermediate calls so you can chain them.

## Watch-outs

- Feature queries are capped at 200 records. If a layer has more features in the
  AOI, tighten the `where` clause or the bbox, and say the result is partial.
- Multi-part geometries are supported (MultiPolygon).
- If any concept is sensitive, stop and route to human review before mapping.
