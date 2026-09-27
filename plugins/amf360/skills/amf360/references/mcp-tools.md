# AMF360+ MCP tools

The AMF360+ MCP server exposes these tools. Connect to it at the `/mcp`
endpoint (Streamable HTTP). All queries are read-only.

## Discovery

- **list_topics** — the 9 topics and 28 subtopics. Start here to orient.
- **discover_layers** `{topicId?, subtopicId?, type?, scope?, withUrlOnly?, activeOnly?, limit?, lang?}`
  — browse indicators by facet. Types: `feature`, `imagery`, `h3`, `component`.
  Scope: `regional` (pan-Amazon) or `ecuador` (national module).
- **find_indicator** `{query, type?, scope?, limit?, lang?}` — ranked keyword
  search over names and descriptions in EN/ES/PT. Deterministic. `scope` filters
  regional vs ecuador. Returns candidate ids.
- **get_indicator** `{id, lang?}` — full catalog record: names, unit,
  topic/subtopic, resource type, scope, live URL, layer id, query_ai template,
  the CARE `sensitive` flag, and the Ecuador `nationalEquivalent` /
  `replacesRegional` cross-reference.
- **get_layer_schema** `{id}` — LIVE ArcGIS schema: fields, geometry type,
  extent, last edit date, copyright. Use to verify a dataset before querying.

## Governance

- **assess_layer** `{id}` — trustworthiness of a layer: freshness (last-edit age
  → fresh / aging / stale / unknown) and credibility (attribution gap, source,
  CARE sensitivity). Rule-based, mirroring the AMF360+ governance agents. Use it
  before trusting a dataset, and cite the result in the answer's provenance.

## Query

- **query_arcgis** `{id, where?, outFields?, aoiId?, spatialFilter?, bbox?,
returnGeometry?, returnCountOnly?, resultRecordCount?, confirmHumanReview?}` —
  read-only query against a feature layer. Pass an `aoiId` (from resolve_area):
  `spatialFilter:"polygon"` (default) clips to the true AOI boundary,
  `"bbox"` uses the cheaper bounding box; or pass a raw `bbox`. Capped at 200
  records. Sensitive layers withhold geometry unless `confirmHumanReview=true`.
  Use `returnCountOnly=true` for cheap aggregate context.

## Analysis

- **intersect_layers** `{aoiId, indicatorIds[], includeGeometry?, confirmHumanReview?}`
  — intersect an AOI with several feature layers in turn (AOI ∩ L1 ∩ L2 ∩ …) and
  report the overlap area, share of the AOI, and a per-layer breakdown. This is
  the multi-condition tool (e.g. deforested ∩ high-biodiversity within the AOI).
  Sensitive layers need `confirmHumanReview=true`.
- **zonal_stats** `{aoiId, id, field?, confirmHumanReview?}` — for one feature
  layer within an AOI: feature count, area covered (km2 and % of the AOI), and
  optional sum/mean/min/max of a numeric `field`. Sensitive layers need
  `confirmHumanReview=true`.

## Geospatial (gazetteer)

- **resolve_area** `{name?, geojson?, country?, includeGeometry?}` — resolve an
  AOI to a polygon and return a reusable **aoiId** plus area/bbox/centroid. Order:
  bundled macro-regions (Amazonia, `'<country> Amazon'` e.g. "Ecuadorian
  Amazon") → reference layers matched by name (municipality, protected area,
  basin, state, country) → Nominatim for precise free-text places. Or pass a
  GeoJSON polygon directly. Reuse the aoiId in the tools below.
- **geocode_place** `{query, country?, limit?}` — geocode free text with
  Nominatim (OSM). Polygon hits come back with an aoiId ready to use; point-only
  hits return a bbox. For precise places outside the reference layers.
- **list_reference_layers** `{}` — the gazetteer's reference layers (role,
  indicator id, geometry, name fields, sensitivity).
- **spatial_intersection** `{aoiId?|aoi, otherAoiId?|other, includeGeometry?}` —
  overlap of two polygons (each an aoiId or a GeoJSON polygon): intersection km2
  and share of the AOI covered.

## Resources

- `amf360://methodology` — the compact methodology (this skill's short form).
- `amf360://catalog` — catalog version and how to explore it.

## Typical call order

`list_topics` or `find_indicator` → `get_indicator` → `get_layer_schema` →
`resolve_area` (get an aoiId) → `query_arcgis` (aoiId filter) →
`spatial_intersection` (aoiId × aoiId) → assemble the report.
