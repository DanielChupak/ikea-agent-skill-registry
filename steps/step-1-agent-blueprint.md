# Step 1 — Trusted Agent Blueprint

## Goal
Model an **agent** in the registry, capturing its purpose, function, ownership, and the variables that feed the health score.

## Problem
Developers can't browse or trust what isn't modeled. We need a blueprint that answers: *What is this agent for? What does it do? Who owns it? How reliable is it?*

## What we build
The `ikea_registry_agent` blueprint.

### Property design
- `purpose` (string, markdown) — the "why": what problem it solves
- `function` (string, markdown) — the "what/how": capabilities and behavior
- `category` (string, enum) — coding, data, devops, security, productivity, other
- `model` (string) — underlying model / runtime (agent-specific)
- `version` (string)
- `lifecycle` (string, enum) — experimental, beta, ga, deprecated
- `vetting_status` (string, enum) — **health variable**: unvetted, in_review, vetted, deprecated
- `success_rate` (number) — **health variable**: observed success/pass rate (%)
- `total_invocations` (number)
- `last_evaluated` (string, date-time)
- `documentation_url` (string, url) — supports top health tier
- `owner` (string) — owning team/person
- `tags` (array)

## MCP command (run for this step only)

```
CallMcpTool
  server: user-port-eu
  toolName: upsert_blueprint
  arguments:
{
  "blueprint": {
    "identifier": "ikea_registry_agent",
    "title": "Trusted Agent",
    "icon": "AI",
    "schema": {
      "properties": {
        "purpose": { "title": "Purpose", "type": "string", "format": "markdown", "description": "Why this agent exists / the problem it solves" },
        "function": { "title": "Function", "type": "string", "format": "markdown", "description": "What it does and how it behaves" },
        "category": { "title": "Category", "type": "string", "enum": ["coding", "data", "devops", "security", "productivity", "other"], "enumColors": { "coding": "blue", "data": "purple", "devops": "orange", "security": "red", "productivity": "green", "other": "lightGray" } },
        "model": { "title": "Model / Runtime", "type": "string" },
        "version": { "title": "Version", "type": "string" },
        "lifecycle": { "title": "Lifecycle", "type": "string", "enum": ["experimental", "beta", "ga", "deprecated"], "enumColors": { "experimental": "yellow", "beta": "blue", "ga": "green", "deprecated": "red" } },
        "vetting_status": { "title": "Vetting Status", "type": "string", "enum": ["unvetted", "in_review", "vetted", "deprecated"], "enumColors": { "unvetted": "red", "in_review": "yellow", "vetted": "green", "deprecated": "darkGray" } },
        "success_rate": { "title": "Success Rate (%)", "type": "number", "description": "Observed success/pass rate" },
        "total_invocations": { "title": "Total Invocations", "type": "number" },
        "last_evaluated": { "title": "Last Evaluated", "type": "string", "format": "date-time" },
        "documentation_url": { "title": "Documentation", "type": "string", "format": "url" },
        "owner": { "title": "Owner", "type": "string" },
        "tags": { "title": "Tags", "type": "array" }
      },
      "required": ["purpose", "vetting_status"]
    },
    "calculationProperties": {},
    "aggregationProperties": {},
    "mirrorProperties": {},
    "relations": {}
  }
}
```

## Checkpoint
- Blueprint `ikea_registry_agent` exists.
- It has `vetting_status` (enum) and `success_rate` (number) — the two health variables.

## Verify
Open Port → Builder → Blueprints → **Trusted Agent**, or re-run:
```
CallMcpTool server: user-port-eu toolName: list_blueprints arguments: { "identifiers": ["ikea_registry_agent"] }
```

## Notes
- `icon` must be a valid Port icon. If `"AI"` is rejected, use `"Service"` or `"DefaultBlueprint"`.
- Want ownership tied to real Port teams instead of a string? Swap `owner` for a relation to `_team` (target `_team`, required false, many false). Kept as a string here for a dependency-free dummy build.

## Pause
> "Agent model is in. Ready for **Step 2 — the Trusted Skill blueprint**?"
