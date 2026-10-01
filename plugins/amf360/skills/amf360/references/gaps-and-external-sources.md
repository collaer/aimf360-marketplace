# Gaps and external sources

AMF360+ data comes first. When the catalog cannot provide something the
answer needs, that is a **gap**. You may fill a gap from outside AMF360+, but
every fill must be visible: the reader must be able to tell, at a glance, which
figures came from AMF360+ and which did not.

## How a gap can be filled

| Filled by          | Meaning                                                                 | `origin` value    |
| ------------------ | ----------------------------------------------------------------------- | ----------------- |
| Another MCP server | A tool from a different MCP server (name the server and the tool).      | `other_mcp`       |
| A website          | Data found on the web (give the URL of the page the figure is on).      | `web`             |
| Model knowledge    | Your own training knowledge, not checked against a source. The weakest. | `model_knowledge` |
| Not filled         | You found no source. Say so; do not guess.                              | `not_filled`      |

Prefer, in order: another MCP server with an authoritative source, then an
official website (a ministry, a statistics office, the original publisher),
then other websites. Use model knowledge only when nothing else is available,
and never for a number the answer depends on without saying so in the
summary.

## Rules

1. **Look in AMF360+ first.** Only call it a gap after `find_indicator` and
   `discover_layers` found nothing suitable.
2. **Declare every gap**, filled or not, in `gaps`:
   `{need, filledBy, source, detail, usedIn}`. `need` says what was missing,
   `detail` says what you used, `usedIn` says where it appears.
3. **Tag every figure with its origin.** Each result row and table carries
   `origin` (default `amf360`) and `originRef` (the server and tool, the URL,
   or what the knowledge is based on).
4. **Never mix silently.** Do not combine an external figure with an AMF360+
   figure in one number (a ratio, a sum) without saying so in the result's
   label and in the gap's `detail`.
5. **Say it in the summary.** If an external figure or an unfilled gap affects
   the answer, the summary says so in plain words.
6. **Lower the confidence** when a key figure is external: at most medium for
   web or other MCP data, at most low for model knowledge.
7. **Keep it in the trace.** Each external lookup is a row in "Executed
   operations and decisions" with the URL or tool, and the decision taken.

## What the report shows

`generate_report` makes the gaps impossible to miss:

- a warning banner right under the question when anything came from outside
  AMF360+ or a gap stayed unfilled;
- a "Gaps and external sources" section right after the summary, with a table
  of every gap and how it was filled, and a list of every external figure;
- a "⚠" mark and an "Origin" column on each external result, and a warning on
  each external table;
- a count of external items and unfilled gaps on the confidence line.

When there are no gaps, the section says so: "No gaps. Every figure in this
report comes from the AMF360+ catalog through its tools."

If you assemble the report yourself (client side), reproduce all of this:
banner, section after the summary, marks on each figure.

## Not a way around the scope gate

Filling gaps applies only to questions that are in scope (see `scope.md`): the
core of the answer still comes from AMF360+. An out-of-scope question gets
only the short decline, with no web search and no answer from knowledge.
