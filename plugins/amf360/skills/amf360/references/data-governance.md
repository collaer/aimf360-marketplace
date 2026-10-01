# Data governance: is each layer current?

For every layer you use in an answer, check whether its data is current or
whether a newer version is published and openly accessible, and give it a
grade. The result goes in the report's "Data governance" section. Do this with
or without the AMF360+ MCP server.

## Two dates that are not the same

- **Service last edit:** when AMF360+ last republished the layer on ArcGIS.
- **Data year:** the year (or version) of the data itself, as stated by the
  original source.

A layer edited in 2025 can hold data from 2020 (for example the oil blocks
layer, #40, built on "RAISG, 2020"). Never judge currency from the service
edit date. Judge it from the data year.

## Method

1. **Read the metadata.** With the MCP server, call `assess_layer {id}`: it
   returns `provenance` (the original source statement, data year, version,
   license, and the URLs cited) and a provisional `currency` grade. Without
   it, open the layer's ArcGIS item page and read "Credits (Attribution)"
   (accessInformation), "Terms of use" (licenseInfo) and the description.
   Ignore AMF360+'s own compilation credit ("Original Compilation
   AmazoniaForever360+ ..., 2023-2025"): its years are not the data's.
2. **Find the data year.** Take it from the source statement first, then the
   layer name. If they disagree (the catalog says "RAISG 2023", the metadata
   says "RAISG, 2020"), report both and use the source statement.
3. **Look for a newer version, starting where the metadata points.** Check
   the URLs cited in the metadata (`provenance.metadataUrls`) first, then the
   original publisher's open data portal (`currency.whereToCheck`, from the
   source registry: Protected Planet for WDPA, gadm.org, raisg.org, the
   Geoportal IDE Ambiente of the Ministerio del Ambiente for the Ecuador
   module, and so on), then other well-known open data portals for the same
   dataset. Stop when you have an answer. Only use public pages: do not
   download data, sign in or register. If a source needs registration (IBAT,
   ACLED), say so and stop there.
4. **Grade the layer** A, B, C or D (see "Grades" below). The MCP server only
   gives a provisional B or C; only you can give A or D, after checking. If
   you could not check (no web access, page down, sign-in needed), keep the
   provisional grade and say why.
5. **Write the note.** One line per layer: the data year, what you checked
   and where, what you found (the newer release's name, date and URL), and
   what it means for the result. Example: "WDPA 2023 in the layer; Protected
   Planet publishes monthly, September 2026 release available. Coverage
   figures may miss areas declared since 2023."
6. **Pass it to the report.** In `generate_report`, give `governance`, one
   entry per layer: `{indicatorId, grade, newerVersion, checked, note}`.
   Without it, the report shows the provisional grade and where to look.

## Grades

| Grade | Meaning                                                                          | Needs                                |
| ----- | -------------------------------------------------------------------------------- | ------------------------------------ |
| **A** | Current: the layer has the latest release published by the source.               | A check at the source.               |
| **B** | Probably current: the data year is within the source's release cycle.            | Metadata only.                       |
| **C** | A newer version is likely (past the release cycle), or the data year is unknown. | Metadata only.                       |
| **D** | Superseded: a newer version is published and openly accessible.                  | A check at the source that found it. |

## What to do with a C or D

- Say it in the summary when it affects the answer.
- Do not swap in the newer data silently. AMF360+ tools can only analyse the
  catalog layers. If you use the newer data anyway (fetched from the web or
  from another MCP server), that is a gap fill: declare it as described in
  `gaps-and-external-sources.md`.
- Recommend updating the layer in AMF360+, naming the newer release.

## Checking the web is allowed here

Checking a layer's source for a newer version is part of the in-scope method.
It is not answering an out-of-scope question, which still gets only a short
decline (see `scope.md`).
