---
name: orphex-reporting
description: >-
  Produce a report from an Orphex MCP connection — a weekly or monthly performance report,
  an executive summary, a creative or account review — by filling one of the report
  templates the workspace can use rather than inventing a structure. Use it whenever someone
  asks for a report, a recap, a summary document or a deck's worth of numbers over Orphex
  data. It carries the workflow: list the report skeletons the workspace is eligible for,
  read the one that fits, fill each section with the measurements it names, then render.
  The skeletons and their section lists live on the server and are read live every time;
  nothing about a specific report is shipped in this file.
---

# Orphex reporting

## When this applies

Someone asks for a *document*: "the weekly report", "a monthly summary for the client", "a
creative review", "a recap of last quarter". The analytical method is `orphex-analyst`'s;
this file is about the shape of the deliverable and where that shape comes from.

The shape is never invented. Orphex publishes report skeletons every workspace can use, an
account or workspace can publish its own beside them, and one of them is almost always closer
to what the reader expects than anything assembled by hand.

## 1. List the skeletons

> skill_catalog lists methods, change procedures and doctrine; skill_read one before building an analysis or change by hand

Call `skill_catalog` with `kind` set to `report_template`. Each row carries a title, a summary
and the phrases that indicate it, filtered to what **this** workspace is eligible for: Orphex's
own templates and, where this connection reaches them, the ones the account or workspace
published. Ids and titles change without this package changing, which is why the list is read
live rather than remembered.

An empty list is an answer, not an error: this workspace has no prepared skeleton, so build
the report from `orphex-analyst`'s method and say plainly that the structure is yours.

## 2. Read the one that fits

Call `skill_read` with the chosen id. It returns the skeleton in full: the sections in order,
whether each is required, and for every value the measurement that fills it, its format,
whether it must be cited, and its `missing` rule for when it comes back empty.

Read it **every time**, immediately before use. Never fill a template from memory of a
previous session, and never carry a section list across workspaces.

## 3. Fill each section with the measurements it names

Work the sections in the order the template gives them; each one narrows what the next needs.
For every section:

1. Find the read that returns the measurements the section names through the ordinary
   loop — `capability_search`, `capability_describe` for its arguments, then `run_read`.
2. Scope every call to the **same window**. Whatever the first call establishes as the
   period is the period for the whole report; a section quietly on a different range is the
   most common way a correct-looking report becomes wrong.
3. Report figures as the capability returned them. Do not convert, re-round, or re-derive.
   Where a period figure has to be formed at all, `orphex-analyst`'s aggregation doctrine
   applies unchanged: additive metrics sum, calculated metrics are Σ ÷ Σ, and a ratio is
   never the average of ratios.
4. Read `request_adjustments` and `as_of` on every result and carry what they say into the
   report, rather than printing the period that was requested.

**A value that comes back empty follows its own `missing` rule.** `stop`: the report cannot be
produced without it — say so rather than render. `show_unavailable`: keep the line and say the
data was not available for the window. `omit`: leave it out. Never estimate a value, and never
carry a placeholder through to the rendered document.

Every number in a finished report must come from a call made in this conversation.

## 4. Render

Produce the finished report on your own document or canvas surface — one document, sections
in the template's order, with its headings. Orphex does not render or send it for you: the
deliverable is yours to present.

State the window and the workspace at the top. Where the report will be read by someone who
was not in the conversation, say which figures were adjusted and when the data was produced.

## Routing map

Report templates are Knowledge Library content, not code, so this map lists none: the
`skill_catalog` call in step 1 is the only list.

<!-- generated: routing -->

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No templates.** Section lists and the measurements they name live in `skill_read`.
- **No schemas or eligibility.** Both are live, per connection and per workspace.
