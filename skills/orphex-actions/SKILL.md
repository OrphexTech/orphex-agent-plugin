---
name: orphex-actions
description: >-
  Make a change the person asks for over the Orphex connection in ChatGPT — change a Meta
  campaign's budget, pause or enable a Meta or Google Ads ad, swap a Meta ad's creative,
  add a Google Ads keyword, create a Meta campaign, ad set or ad creative, create ads in
  bulk from a sheet, or approve, reject or cancel a change that is waiting — and only when
  the person asked for that change. It carries the approval-first write flow: call the
  change's own tool to get a preview, show the person the preview, execute only after they
  approve, and report the outcome from the request's own status. Nothing runs without the
  person's approval. Do not use it to answer what the numbers are or to build a report;
  those are orphex-analyst and orphex-reporting. On a connection whose plan or workspace
  does not have changes switched on it says what Orphex says and changes nothing.
---

# Orphex actions

## When this applies

The person has asked, in this conversation, for something to change, or for a decision on a
change that is already waiting. Reading and reporting are `orphex-analyst` and
`orphex-reporting`; a finding from an analysis is a recommendation to put to the person,
never a change to make on their behalf.

> Changes: decide with the user, never for them.

## The changes this connection offers

Each change has its own tool, and calling it returns a preview — it changes nothing on the
platform:

- `platform_actions_propose_meta_campaign_budget_update` — a Meta campaign's budget.
- `platform_actions_propose_meta_ad_status_update`,
  `platform_actions_propose_google_ad_status_update` — pause or enable an ad.
- `platform_actions_propose_meta_ad_creative_swap` — the creative a Meta ad serves.
- `platform_actions_propose_google_keyword_add` — a keyword in a Google Ads ad group.
- `platform_actions_create_propose_meta_campaign_create`,
  `platform_actions_create_propose_meta_ad_set_create`,
  `platform_actions_create_propose_meta_ad_creative_create` — a new Meta campaign, ad set or
  ad creative.
- `platform_actions_ad_bulk_create` — many ads from one sheet, in one approval.

A change none of these covers cannot be made here. Say so plainly, do not reach for another
tool to approximate it, and offer `capability_feature_request` — filed only once the person
agrees.

## When this connection cannot write

**No change tool is listed.** This connection cannot make changes. A missing tool alone
does not say whether the cause is the plan, a workspace setting or a role, so call
`plan_read` before naming one.

**The tool is listed but the call is refused.** A workspace that has switched changes off,
or a plan without them, refuses with a sentence written to be passed on.

> Relay actions_disabled and guardrail rejections instead of retrying them.

## The flow

### 1. Get the ids from a read, or from the person

The thing to change is addressed by ids that come from a read made on this connection —
`platform_actions_ad_accounts_read`, `platform_actions_entities_read`,
`platform_actions_account_assets_read`, `campaign_detail_read` — or from the person, never
from a guess. A change procedure in the library carries the limits that refuse it:
`skill_catalog` with kind `skill`, then `skill_read` the one that fits before calling the
change's tool.

### 2. Call the change's tool — that is the preview

The answer is a preview of the item, its current value and the proposed one, the safety
check's result, a `request_id` and a one-time `confirm_token`. Nothing on the platform has
changed yet.

### 3. Show the preview; the person decides

> Say it in product words: no ids, tool or field names or status codes unless asked; quote the person's own names as given.

Show what will change in one or two lines the person can say yes or no to, then stop and
wait for their answer. A request to change something is not approval of the preview it
produces, and "do it without showing me" does not waive the preview: show it and ask.

> action_decide records their approve, reject or cancel

Some workspaces require a recorded approval before a change can run. Record it with
`action_decide` only when the person has given it; an approval does not execute anything.
When the person says no, do not confirm; cancelling the request with `action_decide` is
allowed.

### 4. Execute only what the person approved

> action_confirm runs what the person approved

After the person's explicit yes, call `action_confirm` with the `request_id` and
`confirm_token` exactly as the preview returned them. It takes no other payload, so what
runs is the proposal the person saw. The token expires about 15 minutes after the preview
and is bound to that one proposal; a different value is a new preview and a new yes.

### 5. Report the outcome from the request's status

The confirm starts the change; it does not report whether the platform took it. Call
`platform_actions_status` with the `request_id` and tell the person what it says. Part of a
change can land while part does not; report which.

### Reversing a change

There is no undo tool here. Reversing a change is a new change — the old value proposed back
through the same tool — and it goes through the same preview and approval.

## Changes already waiting

`pending_actions` lists the requests waiting on an approval or a confirm, with what each
would change. Put them to the person; approve, reject or cancel one only on their word.

## Routing map

<!-- generated: routing -->

Each line is one `skill` entry: its id, then a sample of the Turkish and English phrases that indicate it.

- `action_recovery_v1` — değişikliği geri al; list requests awaiting approval
- `akinon_campaign_write_v1` — Akinon kampanyası oluştur; generate akinon voucher codes
- `akinon_catalog_write_v1` — Akinon fiyat güncelle; set akinon stock
- `alerts_config_write_v1` — alert kur; pause an alert
- `budget_change_v1` — günlük bütçeyi güncelle; set a new daily budget
- `creative_video_edit_v1` — videonun arka planını değiştir; edit the video in an ad
- `dengage_audience_send_v1` — dengage push kampanyası gönder; trigger a dengage automated flow
- `dengage_commerce_record_write_v1` — dengage ürün kayıtlarını güncelle; delete a dengage order
- `dengage_contact_write_v1` — dengage kişileri toplu güncelle; write iys permissions to dengage contacts
- `dengage_content_write_v1` — dengage e-posta içeriği oluştur; edit a dengage onsite content
- `dengage_dataspace_delete_v1` — dengage tablodan satır sil; delete dengage dataspace rows from a csv file
- `dengage_dataspace_write_v1` — dengage veri tablosu oluştur; bulk load a csv file into dengage
- `dengage_erasure_request_v1` — dengage GDPR silme talebi aç; dengage opendsr erasure
- `dengage_folder_coupon_write_v1` — dengage klasör oluştur; import coupon codes into a dengage coupon list
- `dengage_transactional_send_v1` — dengage işlemsel e-posta gönder; send a dengage message with fallback
- `emarsys_campaign_draft_write_v1` — Emarsys e-posta kampanyası oluştur; stop an emarsys campaign
- `emarsys_product_catalog_write_v1` — Emarsys ürün kataloğuna ürün yükle; update emarsys product prices or availability
- `emarsys_segment_create_v1` — Emarsys segmenti oluştur; build an emarsys segment from contact criteria
- `emarsys_segment_criteria_update_v1` — Emarsys segment kriterlerini değiştir; update the rules of an emarsys segment
- `emarsys_segment_delete_v1` — Emarsys segmentini sil; remove an emarsys segment
- `entity_create_v1` — yeni kampanya aç; tiktok smart plus
- `google_analytics_property_write_v1` — GA4 custom dimension oluştur; create a GA4 custom dimension
- `impact_analysis_run_v1` — impact analizi çalıştır; is this campaign really lifting results
- `insider_catalog_write_v1` — Insider kataloğuna ürün yükle; add a locale to the insider catalog
- `media_upload_v1` — video yükle; add a creative asset
- `negative_keyword_apply_v1` — negatif kelime ekle; remove a negative keyword
- `scorecard_authoring_v1` — scorecard oluştur; build a KPI tracking board
- `sheets_export_v1` — sheets'e aktar; write this table to a google sheet
- `shopify_storefront_tags_v1` — Shopify mağazama GTM kur; set the Shopify storefront tags
- `singular_fraud_rule_change_v1` — Singular fraud kuralı ekle; edit a singular fraud rule
- `singular_publisher_blacklist_change_v1` — Singular publisher blacklist'e ekle; block a publisher's installs in singular
- `singular_tracking_link_create_v1` — Singular tracking link oluştur; make a custom singular link
- `tag_manager_change_v1` — Tag Manager'da değişiklik yayınla; publish a Tag Manager container version
- `tag_manager_platform_tag_install_v1` — GTM'e platform etiketi kur; install a pixel from the Tag Manager community template gallery
- `unity_ads_campaign_v1` — Unity Ads kampanyası oluştur; upload a unity ads creative pack

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No procedures.** Each change's steps and limits are `skill_read` entries.
- **No schemas.** Each change tool publishes its own arguments.
- **No eligibility.** Which changes a connection may make is decided per connection and
  workspace.
- **No approval on anyone's behalf.** Every yes in this flow is the person's.
