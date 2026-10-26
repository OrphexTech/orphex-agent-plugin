---
name: orphex-platform-reads
description: >-
  Route a question that names an advertising platform to the Orphex guide that documents
  that platform's own live reads — Google, Meta, TikTok, LinkedIn, Pinterest, Apple Ads,
  X Ads, Snapchat, OpenAI Ads, Meta organic pages, Yandex, Criteo, GA4, Adjust, AppsFlyer,
  App Store Connect, Google Play, Merchant Center, Search Console, Google Tag Manager,
  Singular, Klaviyo, Shopify, IKAS, Ticimax and T-Soft. Use it when the answer has to be
  what the platform says right now — current accounts and their settings, live entity
  status, fresh performance, recent changes — and use the analyst skill for the method
  that surrounds the call. Each provider's guide states the metric and dimension
  vocabulary it publishes and the shapes it refuses; this file names which one to open.
---

# Orphex platform reads

## When this applies

A question that names a platform, or that only the platform can answer: what an account is
configured to do today, which campaigns exist and what state they are in, what a number
looks like right now, what changed recently. The `orphex-analyst` skill carries the method
around every call and none of it is repeated here. This file answers one question: **which
prepared guide documents this platform's reads.**

## One set of tools, every provider

> Each tool is one operation with its own input schema.

The live reads are the same tools for every provider — `platform_management_accounts_read`,
`platform_management_entities_read`, `platform_management_configuration_read`,
`platform_management_performance_read`, `platform_management_changes_read` and
`platform_management_reporting_vocabulary_read` — with the provider as an argument. What
differs per provider is the vocabulary, the levels it reads at and the shapes it refuses,
and that is what its guide carries.

Stored and live are two different answers. A figure from the platform is current and in the
ad account's own currency; a figure Orphex has stored is normalized across providers. Do not
compare one with the other without saying so.

> skill_read a skill_catalog entry before stating how a number is computed, attributed or denominated.

## Open the guide before the call

The routing map below is pre-knowledge for choosing among the guides without a round-trip.
It is not the authority: eligibility is decided per workspace and the library moves without
this package moving, so `skill_catalog` is what exists and `skill_read` is what it says.
**Always `skill_read` the live entry before following its flow.** A guide this workspace is
not eligible for is withheld whole, which is the connection speaking and not an error to
work around. Where a guide names a step no listed tool performs, say so rather than
guessing.

The **shared** entries, the `platform_management_reads` rows, carry what is true of every
provider — refs, coverage, paging and the account and entity walk — and a provider's own
entry carries only what is different about it. A busy provider is **more than one entry**:
its halves divide on the question a reader arrives with — what an account is set to, what it
did, what changed, how the numbers break down — so pick the half that matches the question.

## Routing map

<!-- generated: routing -->

Each line is one `guide` entry: its id, then a sample of the Turkish and English phrases that indicate it.

- `platform_management_reads_activity_v1` — canlı performans oku; filter performance rows
- `platform_management_reads_structure_v1` — reklam hesaplarını listele; read an account currency and timezone
- `platform_management_reads_v1` — canlı platform okuması; which provider serves what
- `platform_reads_adjust_activity_v1` — install sayısı; mmp attribution read
- `platform_reads_adjust_segments_v1` — Adjust haftalık kırılım; installs by source app
- `platform_reads_adjust_v1` — adjust; adjust partner forwarding
- `platform_reads_akinon_v1` — akinon ürünlerim; connected akinon stores
- `platform_reads_app_store_v1` — App Store Connect uygulamalarını listele; read average App Store rating without review text
- `platform_reads_apple_ads_activity_v1` — Apple Ads performans; Apple Ads async rapor
- `platform_reads_apple_ads_v1` — apple ads; Apple Ads app screenshots and previews
- `platform_reads_appsflyer_activity_v1` — appsflyer performans; revenue by acquisition date
- `platform_reads_appsflyer_v1` — appsflyer; what changed in appsflyer
- `platform_reads_criteo_structure_v1` — Criteo reklamveren listesi; criteo entity settings
- `platform_reads_criteo_v1` — criteo performans; criteo report
- `platform_reads_emarsys_v1` — emarsys campaigns; emarsys segmentleri
- `platform_reads_google_activity_v1` — Google performans oku; which keyword match type performed
- `platform_reads_google_analytics_activity_v1` — analytics oturum verisi; is traffic coming in right now
- `platform_reads_google_analytics_segments_v1` — GA4 hangi metrikleri destekliyor; list ga4 dimensions and metrics
- `platform_reads_google_analytics_v1` — ga4; which app does this GA4 property measure
- `platform_reads_google_assets_v1` — Google başlık hangi pozisyona pinli; which ads use this asset
- `platform_reads_google_changes_v1` — Google değişiklik geçmişi; google recommendations
- `platform_reads_google_conversions_v1` — dönüşüm hedefleri; conversion label
- `platform_reads_google_criteria_v1` — Google kampanya ülke kırılımı; which audience is profitable
- `platform_reads_google_segments_v1` — Google kırılımları neler; placement performance
- `platform_reads_google_studies_v1` — Google deneyi anlamlı mı; did my ads lift awareness
- `platform_reads_google_travel_v1` — Google otel sınıfına göre performans; hotel performance by city
- `platform_reads_google_v1` — google ads; ad approval status google
- `platform_reads_ikas_v1` — ikas; ikas product catalogue
- `platform_reads_klaviyo_v1` — klaviyo campaigns; klaviyo kampanyaları
- `platform_reads_linkedin_activity_v1` — linkedin spend; linkedin ad level report
- `platform_reads_linkedin_v1` — linkedin; linkedin reklamı neden reddedildi
- `platform_reads_merchant_center_structure_v1` — merchant center alt hesapları; merchant center availability
- `platform_reads_merchant_center_v1` — merchant center; merchant center conversion value
- `platform_reads_meta_activity_v1` — Meta performans oku; how many purchases on meta
- `platform_reads_meta_catalog_v1` — meta catalog products; meta katalog feed zamanlaması
- `platform_reads_meta_changes_v1` — Meta değişiklik geçmişi; meta recommendations
- `platform_reads_meta_segments_v1` — Meta kırılımları; meta creative asset breakdown
- `platform_reads_meta_social_v1` — bağlı facebook sayfalarını listele; organic reach and views
- `platform_reads_meta_v1` — meta; meta custom conversions
- `platform_reads_mixpanel_v1` — mixpanel events; mixpanel kayıtlı rapor
- `platform_reads_openai_ads_activity_v1` — OpenAI Ads performansı; openai ads attributed events
- `platform_reads_openai_ads_v1` — OpenAI Ads hesabını listele; openai ads account currency
- `platform_reads_pinterest_v1` — pinterest; pinterest haftalık performans
- `platform_reads_playstore_structure_v1` — Play Store uygulama listesi; play console crash groups
- `platform_reads_playstore_v1` — google play okuması; play store data freshness
- `platform_reads_reddit_structure_v1` — Reddit Ads hesaplarını listele; reddit ad group bid strategy
- `platform_reads_reddit_v1` — Reddit Ads performans; reddit ads change history
- `platform_reads_search_console_v1` — search console; which pages rank
- `platform_reads_shopify_activity_v1` — Shopify satışları; Shopify metrikleri
- `platform_reads_shopify_v1` — shopify; list Shopify segments
- `platform_reads_singular_activity_v1` — Singular install sayısı; singular data freshness
- `platform_reads_singular_segments_v1` — Singular kırılımları; singular filter by app
- `platform_reads_singular_v1` — singular; singular tracking links
- `platform_reads_snapchat_changes_v1` — Snapchat değişiklik geçmişi; what changed on my snapchat campaign
- `platform_reads_snapchat_structure_v1` — Snapchat organizasyon; snapchat custom conversions
- `platform_reads_snapchat_v1` — snapchat performans; snapchat ad account list
- `platform_reads_tag_manager_v1` — GTM container denetimi; is the GA4 tag installed in GTM
- `platform_reads_ticimax_v1` — ticimax siparişlerim; ticimax orders
- `platform_reads_tiktok_activity_v1` — TikTok performans oku; tiktok view-through conversions
- `platform_reads_tiktok_segments_v1` — TikTok ülke kırılımı; tiktok hourly breakdown
- `platform_reads_tiktok_v1` — tiktok; tiktok business center hierarchy
- `platform_reads_tsoft_v1` — t-soft storefront; t-soft iptal iade
- `platform_reads_twitter_conversions_v1` — X Ads dönüşüm sayısı; x ads web event tags
- `platform_reads_twitter_structure_v1` — X Ads hesaplarını listele; which currency is this x ads campaign in
- `platform_reads_twitter_v1` — X Ads performans; x ads clicks and cpc
- `platform_reads_yandex_activity_v1` — yandex performance; yandex harcama
- `platform_reads_yandex_v1` — yandex; yandex kampanya bütçesi

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No flows.** The stepwise bodies live in `skill_read`.
- **No vocabulary.** Which metrics and dimensions a provider publishes is its own guide and
  `platform_management_reporting_vocabulary_read`.
- **No eligibility.** Which platforms a workspace has connected is decided per connection.
- **No method.** Workspaces, windows and aggregation are `orphex-analyst`.
- **No write discipline.** Making a change is `orphex-actions`.
