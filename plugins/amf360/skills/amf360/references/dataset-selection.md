# Choosing the right dataset

The most common failure in geospatial AI is picking a layer because its name
matches a word in the question. Do not do this. Verify every candidate.

## Checklist before you use a layer

For each candidate from `find_indicator`, call `get_indicator` and
`get_layer_schema`, then check:

- **Definition** — what does the indicator actually measure? Read the short and
  full descriptions. "Deforestation" can be annual forest loss, a change between
  two dates, or a restoration-priority mask. These are different questions.
- **Geographic coverage** — does it cover the AOI? A regional Amazon layer and an
  Ecuador-specific layer are not interchangeable.
- **Temporal coverage** — what years? "Change since 2010" needs a layer with a
  2010 baseline and a recent measurement.
- **Spatial resolution / type** — `feature` (vector), `imagery` (raster), or
  `h3` (hex grid). v0 spatial operations use vector feature layers.
- **Units** — km2, %, count, index. Do not compare incompatible units.
- **Source and update date** — from `get_layer_schema` (`editedDate`,
  `copyrightText`). Note when a date is missing; only ~64% of layers expose one.

## Ambiguous concepts

When a term has several valid definitions (classic cases: "high biodiversity",
"fire", "conservation", "water"), do not silently choose. List the candidate
indicators, state how they differ, and either pick the one that fits the user's
intent or ask a short clarifying question.

For "high biodiversity" specifically: it could be species richness, threatened
species richness, endemism, Key Biodiversity Areas, or a named biodiversity
index. Identify which the available data supports, and say which you used.

## Ecuador vs regional layers

The Ecuador country module (scope `ecuador`, ids 200-222) provides live national
layers that replace some regional indicators (for example #216 National System
of Protected Areas replaces the regional #11 Protected Areas; #208 Deforestation
2020-2022 replaces #14). `get_indicator` surfaces this both ways:
`nationalEquivalent` on a regional indicator, `replacesRegional` on an Ecuador
one. When the AOI is in Ecuador, prefer the national layer (richer, national
source) and say so; when the AOI spans several countries, use the regional
layer. Filter with `discover_layers { scope: "ecuador" | "regional" }`.
