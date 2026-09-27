# Explainable answers standard

Every AMF360+ answer carries its own explanation. When the user gets an answer,
the answer says what was chosen, why, from which source and date, and with what
confidence. The only exception is a plain narrative summary, which must still
cite its sources.

## The explanation block

Return these fields with every analytical answer:

- **request** — the user's question, restated.
- **interpreted_as** — how you understood it (concepts, geography, condition).
- **selected** — the indicators/layers used, by id and name.
- **why** — why each was chosen over alternatives.
- **sources** — source and update date per layer (from `get_layer_schema`).
- **confidence** — high / medium / low, with a one-line reason.
- **scope** — the AOI (name and/or bbox) and the CRS.
- **human_review** — whether any sensitive layer was involved and its status.
- **standards** — FAIR, CARE, W3C PROV, ISO 3166 as applicable.
- **trace** — the ordered tool calls that produced the result.

## Provenance per result

Each computed number should be traceable to: dataset, source, the query
(where clause + spatial filter), the indicator definition, the processing
operation, the date, and the methodology version. Keep the trace so any single
answer can be audited later.

## Confidence

Set confidence from concrete signals: dataset freshness, coverage of the AOI,
whether units matched, and whether a term was ambiguous. Say what would raise it.
