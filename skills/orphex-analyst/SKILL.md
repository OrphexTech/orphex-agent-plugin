---
name: orphex-analyst
description: >-
  Answer questions about advertising performance over an Orphex MCP connection — spend,
  ROAS, conversions, pacing, anomalies, cross-platform comparisons — for Google, Meta,
  TikTok, Shopify and the rest of the connected platforms. Use it whenever the Orphex
  connection is available and the question is about what the numbers are, why they moved,
  or what to do about it. It carries the method: how to find a capability before calling
  it, how to address a workspace, why the metric vocabulary is discovered rather than
  guessed, the date window the surface enforces, how evidence is gathered and cross-checked,
  and the aggregation doctrine that keeps a period figure correct. It carries no data, no
  schemas and no flows — those stay live on the server and are always the authority.
---

# Orphex analyst

## When this applies

Any question a person asks about their advertising — what a number is, why it moved, which
platform or campaign is responsible, what to change — where an Orphex MCP connection is
available. This file is *method*. The connection itself is the authority on what exists,
what this workspace may reach and what any of it means.

## The surface: a fixed tool set over a much larger capability set

The connection publishes a small, fixed set of verbs. It does **not** publish one tool per
report, platform or metric. Everything analytical is a **capability id** behind those verbs,
found and run through the loop below. A session that never searches sees a handful of verbs
and no data surface at all.

## 1. Find the capability, then call it

> capability_search, first with no arguments (an overview by family and platform), then narrowed.

Start with `capability_search` and no arguments: the answer is an overview of what this
connection reaches, counted by family and by platform. Narrow from there with `family`,
`platform`, `entity_level` or `action_kind`, or with `q` to match words against ids and
summaries. Run a read-class id with `run_read`, passing the capability's own arguments in
`inputs`, shaped exactly as `capability_describe` returned them.

Describe the selected read for its target workspace before executing it. Echo the describe
response's `data.contract_ref` at the top level of `run_read`; keep target arguments inside
`inputs`. Reuse a reference for the same workspace and current call contract. A refusal's
`error.details.next_call` names the exact recovery call; follow it and preserve field-specific
guidance. A receipt proves contract delivery, not authorization or semantic correctness.

> Use only ids it returns; an id you cannot use is refused like one that does not exist.

A capability this connection cannot use is refused exactly as one that does not exist —
there is a single "not visible" answer for both. A refusal is therefore never evidence that
something exists somewhere else, and never a reason to retry the same guess.

## 2. Name the workspace

> Run: contract_ref and workspace at the top level, the target's own arguments inside inputs.

Every answer belongs to exactly one workspace. A connection bound to several has a default
one, and `capability_search` / `capability_describe` fall back to it — but `run_read` does
not: name the workspace on every call once more than one is bound, or the answer is silently
about the wrong account.

Describe each of the following ids first and echo its `data.contract_ref` on `run_read`:

- `run_read` on `workspaces.list` to see what this connection is bound to.
- `run_read` on `workspace.brief` on first contact with a workspace, before any metric call.
  It says what the workspace is, what is connected to it and what its reporting conventions
  are — which is what stops the first fetch from being a guess.

What is open differs per workspace. Never carry an id, a metric name or an eligibility
conclusion from one workspace to another.

## 3. Vocabulary before numbers

`controller.catalog` before `controller.fetch`, always. Metric and dimension names are
specific to the workspace and to the data level being asked about; they are not a fixed
vocabulary and they are not guessable from the platform's own naming. Read the catalog for
the level you intend to fetch, then fetch only the names it published.

The same rule holds one level up: ask `capability_search` what reads exist before assuming a
question needs `controller.fetch` at all. Many questions have a purpose-built capability that
answers better than a hand-assembled fetch.

## 4. The date window

`date_start` / `date_end` are inclusive `YYYY-MM-DD` and **cannot run past yesterday**. Today
is not a complete day, so the surface will not report it: a later `date_end` is normalized
down rather than refused. A plan with limited reporting history raises older dates to its
window and drops an out-of-window comparison the same way.

Every one of those adjustments is reported back in `request_adjustments`. **Read that field
on every result and disclose what it says.** A period the caller asked for and a period the
data covers are different facts, and a report that silently prints the requested one is
wrong. Results may also be served from cache; `as_of` is when the returned data was produced,
and it belongs in the answer whenever freshness could change the reader's decision.

## 5. The evidence loop

Work in a loop rather than one large query: start broad, drill one step at a time, and let each result decide the next.

1. **Discover the vocabulary** — catalog first, so the fetch asks for names that exist.
2. **Fetch small and targeted.** A narrow, well-scoped read that answers one question beats a
   wide one whose rows still have to be interpreted.
3. **Drill one step at a time.** Let each result choose the next call: workspace → platform →
   campaign → below, never all four at once.
4. **Cross-check before concluding.** Describe each id and echo its `data.contract_ref`. Use `run_read` on `anomaly.read` for what the system already
   flagged as surprising, and on `insights.read` for the standing findings. A driver you
   derived that the platform never flagged, and a flag you cannot reproduce in the numbers,
   are both worth a sentence.
5. **Report contradictions instead of forcing a story.** When two reads disagree, say so and
   say which is closer to the source. A confident wrong narrative costs more than an honest
   open question.
6. **Stop when the evidence answers the question.** More calls after that add cost, not
   confidence.

Never present a number that did not come from a call made in this conversation. No estimate,
no carried-over figure from an earlier session, no placeholder.

## 6. Aggregation doctrine

This is the rule most often broken, and the one that produces a confidently wrong number.

- **Additive metrics sum.** Spend, impressions, clicks, conversions, revenue: a period figure
  is the sum of its days, and a parent's figure is the sum over its children.
- **Calculated metrics are Σ ÷ Σ.** ROAS, CPC, CPA, CTR, CVR, ROI and every other ratio are
  computed over the period as *sum of numerator ÷ sum of denominator*.
- **Never average a ratio.** The mean of daily ROAS values, or of per-campaign CPCs, is not
  the period's ROAS or CPC — it weights a €10 day the same as a €10,000 one. This is true
  across days and across rows alike, and it is wrong in both directions, so it cannot be
  waved away as a rounding difference.
- **Prefer the server's own aggregate.** Ask `controller.fetch` for the period you want and
  report what it returns, rather than fetching daily rows and recomputing. Re-deriving a
  figure the backend already computed is where a rule mismatch enters.
- **Never sum money across currencies.** A set of amounts in more than one currency is not a
  total; report them apart, or ask for the workspace's own reporting figure.

> skill_read one before building an analysis or change by hand, or stating how a number is computed, attributed or denominated.

A stored amount is in the workspace's reporting currency; a live provider read is in the ad account's own.

Before stating how Orphex computes, denominates, refreshes or attributes a number, read the
doctrine entries: `skill_catalog` with kind `guide`, then `skill_read` on the rows whose topic
is `doctrine`. They are the authority on this arithmetic; the paragraph above is the habit
that stops the wrong answer from being given before they are read.

## 7. Answer shape

> Answer with a one-line headline, then drivers each backed by a fetched number, then recommended actions.

Three well-evidenced drivers beat ten half-checked ones. Attach the window and, when it
matters, the `as_of`. Say what the evidence did not settle rather than rounding it away.

## Prepared methods come first

> skill_catalog lists methods, change procedures and doctrine; skill_read one before building an analysis or change by hand

`skill_catalog` and `skill_read` are called directly — they are the door onto the prepared
methods and they do not go through the discovery loop. The routing map below is
pre-knowledge for choosing among them without a round-trip; the server is the authority when
they disagree, because eligibility is per workspace and the corpus changes without this
package changing. **Always `skill_read` the live entry before executing its flow** — the flows
themselves are never shipped here, so this file can go out of date about what exists but can
never assert a stale procedure.

The analyses asked for most are Knowledge Library guides that the routing map below does not
list: investigating an anomaly, a weekly account health review, reallocating budget, creative
fatigue, comparing platforms, pacing against a target, search-term hygiene and brand
incrementality. When a question has one of those shapes, call `skill_catalog` with `kind`
`guide` and `skill_read` the row whose phrases match it before building the analysis by hand.
Their ids and titles can change without this package changing, so find them by intent; never
reuse an id from memory. A family missing from the list is not available to this workspace;
work that question with the evidence loop above instead.

## Routing map

This map lists only the entries that ship with Orphex's code. The method guides above are not
in it; `skill_catalog` is the only way to reach them.

<!-- generated: routing -->

Each line is one `guide` entry: its id, then a sample of the Turkish and English phrases that indicate it.

- `action_handshake_v1` — onay bekleyen değişiklik; approve a pending action
- `ad_policy_status_v1` — reklam onaylandı mı; which headlines were disapproved
- `change_history_read_v1` — ne değişti; who paused my campaign
- `monthly_native_performance_summary_v1` — native monthly performance summary; aylık native performans özeti
- `native_account_measurement_review_v1` — native account measurement review; native hesap ölçüm incelemesi
- `native_pacing_spend_review_v1` — native spend pacing review; native bütçe pacing incelemesi
- `new_account_orientation_v1` — hesabı tanı; how is this account set up
- `orphex_aggregation_doctrine_v1` — toplam roas nasıl hesaplanıyor; what does basis mean
- `orphex_attribution_doctrine_v1` — conversions neyi sayıyor; why is the mmp install count different
- `orphex_currency_doctrine_v1` — kur çevrimi yapılıyor mu; why does orphex differ from the platform
- `orphex_freshness_and_windows_doctrine_v1` — veri ne kadar güncel; what does as_of mean
- `orphex_source_routing_v1` — orphex'te veri yok; which source answers this
- `plan_read_v1` — planım ne; upgrade my plan
- `platform_actions_prewrite_asset_reads_v1` — sayfa akisi; which assets served together
- `platform_actions_prewrite_reads_v1` — anahtar kelime fikirleri; pinterest targeting options
- `platform_actions_prewrite_structure_reads_v1` — urun grubu; which product group is spending
- `safe_actions_usage_v1` — güvenli değişiklik; apply a platform action
- `subscription_economics_v1` — abonelik geliri; subscription retention

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No flows.** The stepwise bodies live in `skill_read` and update server-side.
- **No schemas.** `capability_describe` is the only source for a capability's arguments.
- **No eligibility.** What a workspace may reach is decided per connection and per workspace,
  never here.
- **No data, no ids, no tenant names.** Nothing in this file is a fact about an account.
- **No write discipline.** This is the read method; proposing and confirming a change is a
  separate surface with its own rules.
