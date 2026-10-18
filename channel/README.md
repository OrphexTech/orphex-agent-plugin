# Orphex channel for Claude Code

Pushes what happens in your Orphex workspaces into the Claude Code session you already have
open: a change request waiting for your approval or your confirm, one about to expire or
settled, an ad disapproved, spend running into a cap, a connected ad account that lost
access, a report that finished. Claude tells you what happened and offers the next step
through the Orphex tools. It never approves, rejects or runs a change on its own.

This is a [Claude Code channel](https://code.claude.com/docs/en/channels). It runs on your
machine, reads events from Orphex with the MCP Events extension's `events/poll`, and pushes
each one into the session. It is one-way: there is no reply tool and no remote permission
relay.

## Requirements

- Claude Code with channels (a research preview), signed in with a claude.ai account or a
  Claude Console API key. On Team and Enterprise plans an admin turns channels on
  (`channelsEnabled`) and lists this plugin in `allowedChannelPlugins`.
- Node.js 20 or later on your `PATH`. The plugin has no other dependency.
- An Orphex connection with MCP Apps turned on: Orphex serves events to those connections.

## Install and run

```
/plugin marketplace add OrphexTech/orphex-agent-plugin
/plugin install orphex-channel@orphex
```

Then start Claude Code with the channel turned on. While channels are in research preview a
plugin outside Anthropic's allowlist runs under the development flag:

```
claude --dangerously-load-development-channels plugin:orphex-channel@orphex
```

If your organization lists the plugin in `allowedChannelPlugins`, use
`claude --channels plugin:orphex-channel@orphex` instead.

The first time, a browser window opens to sign in to Orphex: the same Orphex sign-in the
`orphex` plugin uses, returning to port 33418 on your machine. After that the channel
refreshes its own sign-in.

## What it reads

By default it listens to `action.approval_requested`, `action.confirm_requested`,
`action.approval_granted`, `action.expiring`, `action.settled`, `ad.disapproved`,
`budget.constrained`, `connection.access_lost` and `report.finished`, in your connection's
default workspace. To change either, set these before starting Claude Code:

- `ORPHEX_WORKSPACES`: comma-separated workspace references, for example `ws-123,ws-456`.
- `ORPHEX_EVENTS`: comma-separated event names.

An event your role cannot read in a workspace is skipped. Events that happened while Claude
Code was closed are read when it starts again, up to a day back; if some were not kept, the
channel says so.

## Data and security

- It talks to `https://api.orphex.co` only. The sign-in and the position it has read up to
  are stored in the plugin's own data directory on your machine.
- Event text comes from Orphex and your ad platforms. Claude treats it as data, never as
  instructions, and every change still goes through Orphex's preview and your approval.
- Remove it with `/plugin uninstall orphex-channel@orphex`; revoking the Orphex connection
  ends its access at once.
