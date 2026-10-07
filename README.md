# Orphex

Orphex is an advertising-analytics platform. This plugin teaches Claude how to work
with an Orphex connection: how to find the right capability before calling it, how to
address a workspace, how to read the metric vocabulary before the numbers, and which
prepared method answers a question about spend, ROAS, conversions, pacing or anomalies
across the ad, analytics and commerce platforms a workspace has connected. It also
carries the approval-first flow for making a change you ask for, such as a budget, a
bid or a campaign's status: Claude shows you a preview, waits for your approval, and
only then makes the change.

## What the plugin contains

- Four skills, written as plain Markdown instructions:
  - `orphex-actions` — makes a change you ask for, such as pausing or enabling a
    campaign, changing a budget or a bid, creating a campaign, ad set or ad, adding
    negative keywords, uploading media or undoing an earlier change, through the
    approval-first flow described below.
  - `orphex-analyst` — answers questions about what the numbers are, why they moved
    and what to do about it, and routes each one to the prepared method that fits.
  - `orphex-platform-reads` — routes a question that names one advertising platform to
    the guide for that platform's own live reads.
  - `orphex-reporting` — builds a report, such as a weekly or monthly performance
    report, an executive summary or an account review, by filling one of the report
    templates the workspace can use.
- One remote MCP server entry, `orphex`.

It contains no hooks, commands, agents, scripts or executables, installs no packages,
and runs no code on your machine.

## What it connects to

The plugin connects to exactly one endpoint, over HTTPS:
`https://api.orphex.co/api/interaction-gateway/v1/mcp`.

Sign-in uses OAuth through Orphex's Auth0 tenant at `https://auth.orphex.co/`: your
browser opens the Orphex sign-in page and you log in with your own Orphex account. The
plugin carries no password, API key or client secret.

## What data is sent

The requests Claude makes on your behalf — which capability to run, the workspace, and
arguments such as dates and metrics — go only to the Orphex endpoint above, and the
results come back from it. Access is limited to the workspaces your Orphex account can
already see. The plugin itself collects nothing, stores nothing and sends no telemetry.

## How changes are made

Answering questions and building reports use read-only calls. A change happens only when
you ask for one, and the `orphex-actions` skill tells Claude to take the same steps every
time:

1. **Propose.** Claude finds the change on the Orphex server, reads its procedure there
   and proposes it. For most changes the proposal changes nothing: the server answers
   with a preview of the item, its current value and the proposed one, and the result of
   its safety checks.
2. **Your decision.** Claude shows you the preview and waits for your answer. The skill
   tells Claude to go ahead only on your approval and never to approve on your behalf.
3. **Change.** Only after you approve does Claude confirm the change, with a one-time
   token bound to the preview you saw, so what runs is what you approved. The token
   expires about 15 minutes after the proposal.
4. **Outcome.** Claude reads the result back from the change request's own status and
   tells you what happened, including any part that did not go through.

A few changes run in a single call with no separate preview; for those the skill tells
Claude to describe what the call will do and to make it only after you say yes. Undoing a
change is a new proposal that goes through the same preview and approval.

Changes go only through the Orphex endpoint above, and only where Orphex allows them:
your Orphex plan has to include changes, the workspace has to have them switched on
(they stay off until a workspace admin enables them), and your Orphex account needs an
editor role in that workspace. Orphex's safety checks can refuse a change, for example a
budget step larger than one change may take, and some workspaces require a recorded
approval, sometimes from a second person, before a change can run. Where changes are not
available, Claude tells you what the server says and changes nothing.

## Example prompts

Replace the names below with your own campaigns and workspaces.

- "Why did our ROAS on Meta drop last week compared with the week before?" — an
  analysis, answered with the `orphex-analyst` method from read-only calls.
- "Build the monthly performance report for September." — a report, built by
  `orphex-reporting`, which starts from a prepared report template when the workspace
  has one.
- "Which of our TikTok campaigns are active right now, and what budget is each one
  on?" — a live read from the platform itself, routed by `orphex-platform-reads` to
  the TikTok guide.
- "Who paused my Google Ads campaigns this week?" — a read of what changed, combining
  the platform's own change log with changes made through Orphex, routed by
  `orphex-analyst`.
- "Raise the daily budget of the 'Summer Sale' Meta campaign from 100 to 120, and show
  me the change before you apply it." — a change, made by `orphex-actions`: Claude shows
  the before-and-after preview and applies it only after you approve.

## Privacy

Orphex's privacy policy is at
[https://orphex.co/en/privacy-policy](https://orphex.co/en/privacy-policy).

## Requirements

An Orphex account with at least one workspace. Accounts and platform connections are
managed at [orphex.co](https://orphex.co).

## Support

Questions and problems: [support@orphex.co](mailto:support@orphex.co).

## Upload to ChatGPT

This is the OpenAI build of the plugin. Every release of it is attached to this
repository's Releases page as `orphex-openai-vX.Y.Z.zip`, with `plugin.json` at the root of
the archive.

1. Download the ZIP of the release you want from the Releases page.
2. Open [https://platform.openai.com/plugins](https://platform.openai.com/plugins) and
   select **Upload new or existing plugin**.
3. Choose the verified developer identity, select **Upload plugin** and choose the ZIP.
4. Under **MCPs**, connect the `orphex` server and complete the domain verification the
   portal shows.
5. Resolve the automated findings, complete the review details and submit for review.
