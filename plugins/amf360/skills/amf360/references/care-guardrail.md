# CARE guardrail for sensitive data

AMF360+ follows the CARE Principles for Indigenous Data Governance (Collective
benefit, Authority to control, Responsibility, Ethics). Some layers describe
indigenous peoples, including non-contacted peoples. These are sensitive and are
handled with a hard rule.

## The rule

Indicators flagged `sensitive` in the catalog are never auto-published. This
includes indigenous territories, native languages, and layers referencing
Waorani, Chakra, or Tagaeri-Taromenane (non-contacted) peoples.

- You **may** report that such data exists and give aggregate, non-locating
  context (for example a count via `query_arcgis` with `returnCountOnly=true`).
- You **may not** return, map, or publish the geometry of a sensitive layer
  without explicit human approval. The MCP server enforces this: `query_arcgis`
  withholds sensitive geometry unless `confirmHumanReview=true`.

## How to comply

1. When a candidate is flagged `sensitive` (visible in `find_indicator` /
   `get_indicator`), tell the user it requires human review.
2. Provide the aggregate context you are allowed to (counts, presence/absence
   within the AOI at a coarse level).
3. If the analysis genuinely needs the geometry, route it to a human reviewer and
   only proceed with a recorded approval.
4. Record the human-review status in the answer's explanation block.

## Why

Locating non-contacted or vulnerable peoples can cause real harm. The reputational
and ethical stakes are high, so the platform defaults to protection and requires a
person in the loop.
