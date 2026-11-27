---
name: orphex-platform-actions
description: >-
  Route a change the person asked for that names one platform to the Orphex guide that
  documents that platform's own change flows — TikTok, Criteo, Pipedrive, RevenueCat,
  Trendyol, Hepsiburada, n11, Amazon seller and the other providers whose changes have a
  guide of their own — and to the shared guides on reading before a write and on the
  approval handshake. Use it together with orphex-actions, which carries the approval-first
  write flow; this file only names which guide to open. Each guide states the changes it
  covers, the reads its flow starts from, what each change takes and what cannot be taken
  back. On a connection without writes these guides are not listed, and nothing here makes
  a change.
---

# Orphex platform actions

## When this applies

The person has asked for a change on one named platform: a TikTok Business Center member, a
Criteo budget, a Pipedrive deal, a Trendyol price, an Amazon inbound plan. The
`orphex-actions` skill carries the write flow around every such change — finding the write,
proposing it, showing the preview, waiting for the person's decision, confirming, and
reporting the outcome — and none of it is repeated here. This file answers one question:
**which prepared guide documents this platform's changes.**

## Open the guide before the change

> skill_catalog lists methods, procedures and doctrine; skill_read one before building an analysis or change by hand

The routing map below is pre-knowledge for choosing among the guides without a round-trip.
It is not the authority: what a connection may change is decided per workspace and plan,
and the guides move without this package moving, so `skill_catalog` is what exists and
`skill_read` is what it says. **Always `skill_read` the live entry before proposing its
change.** A plan without writes lists none of these guides, which is the connection
speaking and not an error to work around.

A platform whose reads and changes are documented apart has its read guide under
`orphex-platform-reads` and its change guide here; the change guide names the reads its
flow starts from, so read those first when it says to.

## Every change keeps the write discipline

> Write: action_propose per write_protocol.

> Decide with the user, never for them: show the preview or approval request; action_decide records their approve, reject or cancel.

The rest of that discipline — the preview, the one-time confirmation, the outcome read from
the request's own status, and undoing through a new proposal — is `orphex-actions`.

## Routing map

<!-- generated: routing -->

Each line is one `guide` entry: its id, then a sample of the Turkish and English phrases that indicate it.

- `action_handshake_v1` — onay bekleyen değişiklik; ask a reviewer
- `braze_reads_v1` — braze ile neler yapılabilir; braze official mcp server
- `challenger_agent_v1` — itirazcı ajan yaz; leave a note on a proposal waiting for approval
- `netmera_lanes_v1` — netmera ile neler yapılabilir; netmera campaign list
- `platform_actions_amazon_seller_account_actions_writes_v1` — amazon tedarik noktası oluştur; create an amazon action plan
- `platform_actions_amazon_seller_external_fulfillment_writes_v1` — amazon depo stok güncelle; reschedule an amazon service appointment
- `platform_actions_amazon_seller_warehousing_writes_v1` — amazon inbound plan oluştur; awd replenishment
- `platform_actions_amazon_seller_writes_v1` — amazon fiyatını güncelle; create amazon a+ content
- `platform_actions_criteo_writes_v1` — Criteo değişikliği öner; propose a criteo change
- `platform_actions_hepsiburada_writes_v1` — hepsiburada fiyat güncelle; create hepsiburada products
- `platform_actions_n11_writes_v1` — n11 fiyat güncelle; send an n11 invoice link
- `platform_actions_pipedrive_crm_admin_v1` — assign a user to a Pipedrive role; Pipedrive takımı oluştur
- `platform_actions_pipedrive_crm_management_v1` — delete a Pipedrive deal; Pipedrive kaydını sil
- `platform_actions_pipedrive_crm_relations_v1` — merge two Pipedrive deals; upload a file to a Pipedrive deal
- `platform_actions_pipedrive_crm_schema_v1` — create a Pipedrive custom field; Pipedrive alan seçeneği ekle
- `platform_actions_pipedrive_crm_structure_v1` — add a stage to a Pipedrive pipeline; Pipedrive lead etiketi oluştur
- `platform_actions_pipedrive_crm_v1` — propose a Pipedrive deal change; Pipedrive kuruluş ilişkisini güncelle
- `platform_actions_prewrite_asset_reads_v1` — sayfa akışı; which assets served together
- `platform_actions_prewrite_reads_v1` — anahtar kelime fikirleri; pinterest targeting options
- `platform_actions_prewrite_structure_reads_v1` — ürün grubu; which product group is spending
- `platform_actions_revenuecat_catalog_writes_v1` — RevenueCat yetki adını değiştir; create a revenuecat entitlement
- `platform_actions_tiktok_catalog_video_v1` — TikTok katalog videosu içe aktar; delete a tiktok catalog video
- `platform_actions_tiktok_writes_v1` — TikTok değişikliği öner; propose a tiktok change
- `platform_actions_trendyol_writes_v1` — trendyol fiyat güncelle; send a trendyol invoice link
- `safe_actions_usage_v1` — güvenli değişiklik; apply a platform action

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No flows.** The stepwise bodies live in `skill_read` and update server-side.
- **No write discipline of its own.** Proposing, deciding and confirming a change are
  `orphex-actions`.
- **No reads.** What a platform says right now is `orphex-platform-reads`.
- **No eligibility.** Which platforms a workspace has connected, and whether its plan and
  settings allow changes, is decided per connection, never here.
