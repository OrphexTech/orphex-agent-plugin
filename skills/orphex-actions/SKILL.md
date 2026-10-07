---
name: orphex-actions
description: >-
  Make a change the person asks for over an Orphex MCP connection — pause or enable a
  campaign, change a budget or a bid, create a campaign, ad set or ad, add negative
  keywords, upload media, undo an earlier change, or approve, reject or
  cancel a change that is waiting — and only when the person asked for that change. It
  carries the approval-first write flow: find and describe the write, propose it, show
  the person the preview, execute only after they approve, report the outcome from the
  request's own status, and undo through a new proposal. Nothing runs without the
  person's approval. Do not use it to answer what the numbers are or to build a report;
  those are orphex-analyst and orphex-reporting. On a connection whose plan or workspace
  does not have writes switched on it says what the server says and changes nothing.
---

# Orphex actions

## When this applies

The person has asked, in this conversation, for something to change: a campaign's status,
a budget, a bid, a new entity, an upload, an undo, or a decision on a change that
is already waiting — anything that runs through `action_propose`. Reading and reporting are `orphex-analyst` and `orphex-reporting`;
nothing here applies to a question that only asks what the numbers are, and a finding from
an analysis is a recommendation to put to the person, never a change to make on their
behalf.

## When this connection cannot write

The write tools are listed when the account's plan includes writes and a bound workspace
has them switched on. So there are two shapes of "no", and neither is a problem to solve
around.

**`action_propose` is not among this connection's tools.** This connection cannot make
changes. Say so, and do not look for another tool to make the change with. A missing tool
alone does not say whether the cause is the plan, a role, a workspace setting or the
provider, so read the plan before naming one.

> For plans or missing writes, find and run plan.read before explaining access.

**The tool is listed but the call is refused.** A workspace that has switched writes off,
or a plan without them, refuses the call with a sentence written to be passed on: it says
nothing was changed, and whether the fix is the plan or a workspace setting a workspace
admin can switch back on. Pass that on as it reads.

> relay actions_disabled and guardrail rejections instead of retrying them.

## The flow

Every step below is the person's to decide and yours to carry out. The procedure for a
given change — which door, which ids, which limits refuse it — is not in this file; it is
the live entry you read in step 1.

### 1. Find the write, describe it, read its procedure

> capability_search, first with no arguments (an overview by family and platform), then narrowed.

The change has its own capability: each platform and each action has its own propose door,
and a new entity has its own create door. Describe the one you found for the workspace the
person means, and read `write_protocol` off that answer before anything else.

> Run: contract_ref and workspace at the top level, the target's own arguments inside inputs.

> skill_catalog lists methods, change procedures and doctrine; skill_read one before building an analysis or change by hand

The routing map below lists the change procedures. `skill_read` the one that fits before
proposing; `safe_actions_usage_v1` and `action_handshake_v1` are the guides for the steps
every write shares. The id of the thing to change comes from a read made on this connection
or from the person, never from a guess.

### 2. Propose — `write_protocol` decides what that does

> Write: action_propose; write_protocol decides what happens.

> two_step returns a preview and confirm_token, and only action_confirm executes it.

> preview_default previews until you propose again with its own apply argument set.

> single_call executes on propose, so check what it spends first.

A `two_step` or `preview_default` propose changes nothing on the platform; the second step
is the change. A `single_call` propose is the change itself and returns no preview, so tell
the person what the call will do before you make it, and make it only once they say yes.

### 3. Show the preview; the person decides

> Decide with the user, never for them: show the preview or approval request; action_decide records their approve, reject or cancel.

Show what will change — the entity, its current value and the proposed one — and the
safety check the preview reports, in one or two lines the person can say yes or no to.

> Say it in product words: no ids, tool or field names or status codes unless asked; quote the person's own names as given.

Some workspaces require a recorded approval before a change can run. Record it with
`action_decide` only when the person has given it; an approval does not execute anything,
and the confirm in step 4 still follows. When a second person is required, the one who
proposed cannot approve it: name the role the request is waiting on, never a person.

### 4. Execute only what the person approved

Call `action_confirm` with the `request_id` and `confirm_token` exactly as the propose
returned them, and with the same workspace. It takes no other payload, so what runs is the
proposal the person saw. The token expires about 15 minutes after the propose and is bound
to that one proposal; a different value is a new proposal and a new yes. If it expired,
`skill_read` `action_handshake_v1` for whether to reissue it or propose again — never send
a dead token twice.

### 5. Report the outcome from the request's status

The confirm starts the change; it does not report whether the platform took it. Describe
`platform_actions.status_read`, run it with `run_read` on the `request_id`, and tell the
person what that answer says. Part of a change can land while part does not; report which.

### 6. Undo is a new proposal

`platform_actions.revert`, opened through `action_propose` with the `request_id` of a
change that succeeded, previews the undo. It is a change like any other: steps 3 to 5 apply
to it unchanged, it faces the same safety checks, and nothing is undone until the person
approves it and it is confirmed. Not every change can be undone; the revert's own answer
says when one cannot.

## Changes already waiting

> If pending_actions is listed, call it at session start.

It lists the requests waiting on an approval or on a confirm, with what each would change.
Put them to the person; approve, reject or cancel one only on their word.

## When a step is refused

> follow error.details.next_call exactly;

A safety check that refuses a change is reported to the person with what stopped it — for
example, a budget step larger than one change may take. A smaller change is a new proposal
they approve, not a way around the check.

## Routing map

<!-- generated: routing -->

Each line is one `skill` entry: its id, then a sample of the Turkish and English phrases that indicate it.

- `action_recovery_v1` — değişikliği geri al; list requests awaiting approval
- `alerts_config_write_v1` — alert kur; pause an alert
- `budget_change_v1` — günlük bütçeyi güncelle; set a new daily budget
- `creative_video_edit_v1` — videonun arka planını değiştir; edit the video in an ad
- `entity_create_v1` — yeni kampanya aç; tiktok smart plus
- `google_analytics_property_write_v1` — GA4 custom dimension oluştur; create a GA4 custom dimension
- `impact_analysis_run_v1` — impact analizi çalıştır; is this campaign really lifting results
- `insider_catalog_write_v1` — Insider kataloğuna ürün yükle; add a locale to the insider catalog
- `media_upload_v1` — video yükle; add a creative asset
- `negative_keyword_apply_v1` — negatif kelime ekle; remove a negative keyword
- `scorecard_authoring_v1` — scorecard oluştur; build a KPI tracking board
- `sheets_export_v1` — sheets'e aktar; write this table to a google sheet
- `singular_fraud_rule_change_v1` — Singular fraud kuralı ekle; edit a singular fraud rule
- `singular_publisher_blacklist_change_v1` — Singular publisher blacklist'e ekle; block a publisher's installs in singular
- `singular_tracking_link_create_v1` — Singular tracking link oluştur; make a custom singular link
- `tag_manager_change_v1` — Tag Manager'da değişiklik yayınla; publish a Tag Manager container version

`skill_catalog` — not this list — is the authority on what exists here, and it returns each entry's summary and the capabilities its flow uses. Always `skill_read` the live entry before executing its flow.

<!-- /generated: routing -->

## What this file deliberately does not carry

- **No procedures.** Each change's steps, ids and limits are `skill_read` entries and
  update server-side.
- **No schemas.** `capability_describe` is the only source for a write's arguments and its
  `write_protocol`.
- **No eligibility.** Which changes this connection may make is decided per connection and
  per workspace, never here.
- **No approval on anyone's behalf.** Every yes in this flow is the person's.
