---
name: orphex-analyst
description: >-
  Answer questions about advertising performance over the Orphex connection in ChatGPT —
  spend, ROAS, conversions, pacing, anomalies, cross-platform comparisons — for Google,
  Meta, TikTok, Shopify and the rest of the connected platforms. Use it whenever the
  Orphex connection is available and the question is about what the numbers are, why they
  moved, or what to do about it. It carries the method: which Orphex tool answers which
  question, how to address a workspace, why the metric vocabulary is read rather than
  guessed, the date window the tools enforce, how evidence is gathered and cross-checked,
  and the aggregation doctrine that keeps a period figure correct. It carries no data, no
  schemas and no flows — those stay live on the server and are always the authority.
---

# Orphex analyst

## When this applies

Any question a person asks about their advertising — what a number is, why it moved, which
platform or campaign is responsible, what to change — where the Orphex connection is
available. This file is *method*. The connection itself is the authority on what exists,
what this workspace may reach and what any of it means.

## The tools

> Each tool is one operation with its own input schema.

Every Orphex tool does one thing and publishes its own arguments. Pick the tool by the
question, and fill its arguments from its own schema and field descriptions; nothing has to
be looked up first.

- **Ad platform numbers:** `platform_management_performance_read` for a report that fits in
  one answer. A large one runs as a job: `platform_management_report_start`, then
  `platform_management_report_status` until it is ready.
- **What a provider reports on:** `platform_management_reporting_vocabulary_read` lists the
  metric and dimension names one provider publishes.
- **Accounts and entities:** `platform_management_accounts_read`,
  `platform_management_entities_read`, `platform_actions_ad_accounts_read` and
  `platform_actions_entities_read` list what is connected and what it contains.
- **One entity in full:** `campaign_detail_read`, `ad_set_detail_read`, `ad_detail_read`,
  `ad_creative_detail_read`, `ad_video_detail_read`, `platform_actions_ad_set_config_read`,
  `platform_actions_ad_texts_read`, `platform_actions_account_assets_read` and
  `platform_management_configuration_read`.
- **What changed:** `platform_management_changes_read`.
- **Analytics and subscriptions:** `google_analytics_report_read`; `revenuecat_chart`, after
  `revenuecat_chart_options` for what a chart can be split and filtered by.

A question none of these answers is not a reason to improvise one from another tool: say
what is missing, and offer `capability_feature_request` once the person agrees.

## 1. Name the workspace

> Several: the person picks one, not you.

Every answer belongs to exactly one workspace. On first contact call `get_workspaces`; when
the connection is bound to more than one, ask which one the question is about and pass that
workspace on every call. `workspace_config` says what that workspace has connected and
whether each connection is healthy, which stops the first read from being a guess.

What is open differs per workspace. Never carry an id, a metric name or a conclusion from
one workspace to another.

## 2. Vocabulary before numbers

Metric and dimension names are specific to the provider and the level being read; they are
not guessable from another platform's naming. Read
`platform_management_reporting_vocabulary_read` for the provider, or the field descriptions
of the tool itself, and ask only for names they publish.

## 3. The date window

Ask for complete days. Today is not complete, so a figure "including today" is a figure for
a day still moving: report through yesterday and say so. A plan with limited reporting
history can narrow an older window. Whatever a result says it adjusted — the window, a
dropped comparison — goes into the answer, because the period asked for and the period the
data covers are different facts. When a result says when its data was produced, carry that
whenever freshness could change the reader's decision.

## 4. The evidence loop

Work in a loop rather than one large query: start broad, drill one step at a time, and let
each result decide the next.

1. **Fetch small and targeted.** A narrow read that answers one question beats a wide one
   whose rows still have to be interpreted.
2. **Drill one step at a time:** workspace → platform → campaign → below, never all four at
   once.
3. **Cross-check before concluding.** Where two reads cover the same figure — an ad platform
   and the analytics property, or two levels of the same account — compare them, and read
   the change history for what moved it.
4. **Report contradictions instead of forcing a story.** Say which read is closer to the
   source.
5. **Stop when the evidence answers the question.**

Never present a number that did not come from a call made in this conversation. No
estimate, no carried-over figure from an earlier session, no placeholder.

## 5. Aggregation doctrine

- **Additive metrics sum.** Spend, impressions, clicks, conversions, revenue.
- **Calculated metrics are Σ ÷ Σ.** ROAS, CPC, CPA, CTR, CVR and every other ratio are
  computed over the period as sum of numerator ÷ sum of denominator.
- **Never average a ratio.** The mean of per-campaign CPCs is not the account CPC — it weights
  a €10 campaign the same as a €10,000 one.
- **Prefer the tool's own aggregate.** Ask for the period you want rather than fetching
  daily rows and recomputing.
- **Never sum money across currencies.** Report amounts in different currencies apart.

> skill_read a skill_catalog entry before stating how a number is computed, attributed or denominated.

Before stating how Orphex computes, denominates or attributes a number, call
`skill_catalog` with kind `guide` and `skill_read` the rows whose topic is `doctrine`. They
are the authority on this arithmetic.

## 6. Answer shape

> Answer with a one-line headline, drivers each backed by a fetched number.

> Say it in product words: no ids, tool or field names or status codes unless asked; quote the person's own names as given.

Three well-evidenced drivers beat ten half-checked ones. Attach the window, and say what the
evidence did not settle rather than rounding it away. A finding is a recommendation to put
to the person, never a change to make on their behalf.

## Prepared methods come first

The analyses asked for most — investigating an anomaly, reallocating budget, comparing
platforms, search-term hygiene — are Knowledge Library guides. When a question has one of
those shapes, call `skill_catalog` with kind `guide` and `skill_read` the row whose phrases
match it before building the analysis by hand. Their ids can change without this package
changing, so find them by intent. **Always `skill_read` the live entry before following its
flow.** Where a guide names a step no listed tool performs, say so rather than guessing.

## Routing map

This map lists only the entries that ship with Orphex's code; `skill_catalog` is the only
way to reach the rest.

<!-- generated: routing -->

Each line is one `guide` entry: its id, then a sample of the Turkish and English phrases that indicate it.

- `action_handshake_v1` — onay bekleyen değişiklik; approve a pending action
- `ad_policy_status_v1` — reklam onaylandı mı; which headlines were disapproved
- `brand_memory_usage_v1` — marka hafızası; which notes apply to this change
- `change_history_read_v1` — ne değişti; who paused my campaign
- `creative_fatigue_diagnosis_v1` — kreatif yorgunluğu; creative fatigue
- `ga4_ads_audience_health_v1` — GA4 kitleleri Google Ads'te 0 görünüyor; remarketing list is empty
- `monthly_native_performance_summary_v1` — native monthly performance summary; aylık native performans özeti
- `native_account_measurement_review_v1` — native account measurement review; native hesap ölçüm incelemesi
- `native_pacing_spend_review_v1` — native spend pacing review; native bütçe pacing incelemesi
- `new_account_orientation_v1` — hesabı tanı; what is connected in this workspace
- `orphex_aggregation_doctrine_v1` — toplam roas nasıl hesaplanıyor; what does basis mean
- `orphex_attribution_doctrine_v1` — conversions neyi sayıyor; why is the mmp install count different
- `orphex_currency_doctrine_v1` — kur çevrimi yapılıyor mu; why does orphex differ from the platform
- `orphex_evidence_and_claims_doctrine_v1` — bu rakam ölçülmüş mü yoksa varsayım mı; is this measured or assumed
- `orphex_freshness_and_windows_doctrine_v1` — veri ne kadar güncel; what does as_of mean
- `orphex_source_routing_v1` — orphex'te veri yok; which source answers this
- `plan_read_v1` — planım ne; upgrade my plan
- `platform_actions_prewrite_asset_reads_v1` — sayfa akışı; which assets served together
- `platform_actions_prewrite_reads_v1` — anahtar kelime fikirleri; pinterest targeting options
- `platform_actions_prewrite_structure_reads_v1` — ürün grubu; which product group is spending
- `read_guidance_v1` — bir sonraki adımı öner; next step after this report
- `roas_drop_diagnosis_v1` — ROAS düştü; how do I diagnose a ROAS drop
- `routine_budget_ceiling_check_v1` — bütçe sınırı rutini; google account budget running out check
- `routine_disapproved_ads_check_v1` — reddedilen reklam rutini; scheduled check for rejected ads
- `routine_external_change_check_v1` — platform dışı değişiklik rutini; daily change history check
- `routine_scorecard_off_track_check_v1` — scorecard hedef rutini; check my kpi targets every week
- `routines_runner_v1` — rutinlerimi çalıştır; what routines are due now
- `safe_actions_usage_v1` — güvenli değişiklik; apply a platform action
- `subscription_customers_audiences_v1` — revenuecat müşteri ayrıntısı; revenuecat audience preview
- `subscription_economics_v1` — abonelik geliri; subscription retention
- `subscription_experiments_targeting_v1` — revenuecat deney sonuçları; revenuecat native chart metadata

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No flows.** The stepwise bodies live in `skill_read` and update server-side.
- **No schemas.** Each tool publishes its own arguments.
- **No eligibility.** What a workspace may reach is decided per connection and workspace.
- **No data, no ids, no tenant names.**
- **No write discipline.** Making a change is `orphex-actions`.
