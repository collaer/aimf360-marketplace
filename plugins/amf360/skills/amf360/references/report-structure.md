# Final report structure

Every AMF360+ analysis ends with a report. It must contain the sections below,
in this order. A section can be short, but it is never silently dropped: if
there is nothing to put in it, say so in one line.

## Required sections

1. **Question and interpretation.** The question as asked and how you
   understood it (concepts, geography, condition).
2. **Summary.** The answer in a few sentences, with the key numbers and their
   units. Someone who reads only this part must get the answer, including
   whether any key figure comes from outside AMF360+.
3. **Gaps and external sources.** Right after the summary, so it cannot be
   missed: every gap (what AMF360+ could not provide) and how it was filled
   (another MCP server, a website, model knowledge, or not filled), and every
   figure from outside AMF360+. When there are none, say "No gaps". If
   anything came from outside, a warning banner also sits under the question.
   See `gaps-and-external-sources.md`.
4. **Overview map of the area.** One map of the chosen area (the AOI) with the
   Ecuador Amazon outline for context. Always included when there is an AOI.
   For an area in Ecuador it carries a locator inset in the top-right corner:
   the whole Ecuadorian Amazon, the national borders of Ecuador, Colombia and
   Peru, and the area of interest marked in red.
5. **Indicator maps.** At least one thematic map, choosing what fits:
   - **one map per indicator**, when each layer matters on its own;
   - **a crossed map**, when the question combines conditions: the layers
     together with their overlap (AOI ∩ A ∩ B) highlighted.

   Use both kinds when it helps. Each map has a legend saying what is drawn,
   how many features, and anything capped or withheld.

6. **Results and data tables.** The key results (metric, value, unit,
   origin), plus any data tables that support them (per zone, per feature,
   per year). Numbers the AMF360+ tools returned, or declared gap fills
   flagged with their origin. Never an unflagged estimate.
7. **Sources.** Per indicator: id, name, scope (regional or Ecuador), the
   AMF360+ service, the original source its metadata cites, license, and
   sensitivity.
8. **Data governance.** Per layer: data year (not the service edit date),
   whether a newer version is published and where you checked, and an A-D
   grade with a one-line note. See `data-governance.md`.
9. **Traceability.**
   - **Planned operations:** the plan from the analysis contract, in order.
   - **Executed operations and decisions:** each tool call that produced the
     result, its key inputs, what it returned, and the action or decision you
     took from it (for example "national layer preferred over regional",
     "AOI simplified", "record cap hit, result partial", "sent to human
     review").
10. **Limitations.** Coverage gaps, record caps, simplifications, data you
    could not verify.
11. **Confidence, human review and standards.** Confidence with its reason;
    human review, which is always recommended (never "not required"), with
    the specific points to check when there are any (CARE layers, figures
    from outside AMF360+, unfilled gaps, layers graded C or D); and the
    standards applied (FAIR, CARE, W3C PROV, ISO 3166).

Sensitive (CARE) layers appear in the sources and maps legend, but their
geometry is not drawn unless a human confirmed it (`confirmHumanReview=true`).

## Keep the trace as you go

From the first tool call, keep a running list of what you ran: operation,
input, result, decision. It feeds section 7. Do not rebuild it from memory at
the end.

## Two ways to build the report

### A. On the server: `generate_report`

Pass everything in one call and the server assembles the report:

- `summary`, `aoiId` (gives the overview map), `indicatorIds`;
- `maps`: up to 4, each `{title, indicatorIds, showIntersection?}`. One
  indicator per map for per-indicator maps; several with
  `showIntersection=true` for a crossed map (the server computes the overlap
  for at most 2 crossed maps per report; use `intersect_layers` for more);
- `results`, `tables` (each with `origin`/`originRef` when not from AMF360+),
  `gaps`, `governance`, `plan`, `trace`, `limitations`, `confidence`.

The server reads each layer's update date and attribution live for the sources
table, and adds its own map steps to the trace. Use this by default.

### B. On the client: assemble it yourself

Use this when the client can render rich output (an artifact, a document, an
HTML page) or when the report must combine results from several sessions.

1. Call `render_map` with the `aoiId` and no layers for the overview map.
2. Call `render_map` again per indicator, and with several `indicatorIds` and
   `showIntersection=true` for a crossed map. Each call returns the SVG, a
   text legend and a `trace` entry; add that entry to your trace.
3. Get each layer's provenance and provisional grade from `assess_layer`,
   then check the source for a newer version (`data-governance.md`).
4. Write the report with the same sections, in the same order, as above.
   Embed the SVGs as they are.

Both ways must produce the same sections. Never drop the overview map, the
sources table or the traceability section because you built the report
yourself.
