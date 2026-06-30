# Step 0 — Orientation & Connectivity

## Goal
Confirm the Port EU MCP server is reachable and that we won't collide with existing blueprints before we build anything.

## Problem
Everything in this workshop runs through the `user-port-eu` MCP server. If it isn't connected, nothing downstream will work. We also want to confirm `ikea_registry_agent` and `ikea_registry_skill` don't already exist (and if they do, decide to reuse/extend rather than overwrite).

## What we build
Nothing yet — this is a read-only connectivity check.

## MCP commands (run for this step only)

Confirm connectivity and list current blueprints (summary):

```
CallMcpTool
  server: user-port-eu
  toolName: list_blueprints
  arguments: {}
```

(Optional) Inspect details if the registry blueprints already exist:

```
CallMcpTool
  server: user-port-eu
  toolName: list_blueprints
  arguments: { "identifiers": ["ikea_registry_agent", "ikea_registry_skill"] }
```

## Checkpoint
- The tool returns a list of blueprints (connectivity OK).
- `ikea_registry_agent` and `ikea_registry_skill` are **not** present — or, if they are, agree with the user to reuse/extend them instead of overwriting.

## If the MCP call fails
- Make sure the Port EU MCP server is enabled in Cursor (see `README.md`).
- Re-run `list_blueprints`. Do not proceed until it succeeds.

## Pause
> "Connectivity confirmed and no name clashes. Ready for **Step 1 — the Trusted Agent blueprint**?"
