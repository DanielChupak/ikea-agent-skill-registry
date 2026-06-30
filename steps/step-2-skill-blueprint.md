# Step 2 — Trusted Skill Blueprint

## Goal
Model a **skill** as a first-class, *separate* type so developers can clearly tell skills apart from agents.

## Problem
The user story is explicit: developers must **differentiate between agents and skills**. The cleanest way to do that in Port is a parallel blueprint with skill-specific framing (a skill is typically invoked/triggered, not autonomous like an agent).

## What we build
The `ikea_registry_skill` blueprint — parallel to the agent blueprint so the health scorecard logic mirrors cleanly.

### Property design
- `purpose` (string, markdown) — what the skill is for
- `function` (string, markdown) — what it does
- `trigger` (string) — how it's invoked (e.g. slash command, keyword, tool call) — **skill-specific**, contrasts with an agent's autonomy
- `category` (string, enum) — coding, data, devops, security, productivity, other
- `version` (string)
- `lifecycle` (string, enum) — experimental, beta, ga, deprecated
- `vetting_status` (string, enum) — **health variable**: unvetted, in_review, vetted, deprecated
- `success_rate` (number) — **health variable**
- `total_runs` (number)
- `last_evaluated` (string, date-time)
- `documentation_url` (string, url)
- `owner` (string)
- `tags` (array)

## MCP command (run for this step only)

```
CallMcpTool
  server: user-port-eu
  toolName: upsert_blueprint
  arguments:
{
  "blueprint": {
    "identifier": "ikea_registry_skill",
    "title": "Trusted Skill",
    "icon": "Book",
    "schema": {
      "properties": {
        "purpose": { "title": "Purpose", "type": "string", "format": "markdown", "description": "What this skill is for" },
        "function": { "title": "Function", "type": "string", "format": "markdown", "description": "What it does" },
        "trigger": { "title": "Trigger / Invocation", "type": "string", "description": "How the skill is invoked (slash command, keyword, tool call)" },
        "category": { "title": "Category", "type": "string", "enum": ["coding", "data", "devops", "security", "productivity", "other"], "enumColors": { "coding": "blue", "data": "purple", "devops": "orange", "security": "red", "productivity": "green", "other": "lightGray" } },
        "version": { "title": "Version", "type": "string" },
        "lifecycle": { "title": "Lifecycle", "type": "string", "enum": ["experimental", "beta", "ga", "deprecated"], "enumColors": { "experimental": "yellow", "beta": "blue", "ga": "green", "deprecated": "red" } },
        "vetting_status": { "title": "Vetting Status", "type": "string", "enum": ["unvetted", "in_review", "vetted", "deprecated"], "enumColors": { "unvetted": "red", "in_review": "yellow", "vetted": "green", "deprecated": "darkGray" } },
        "success_rate": { "title": "Success Rate (%)", "type": "number" },
        "total_runs": { "title": "Total Runs", "type": "number" },
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
- Both `ikea_registry_agent` and `ikea_registry_skill` exist.
- The skill blueprint has a `trigger` property that the agent blueprint does not — reinforcing the agent vs skill distinction.

## Notes
- If `"Book"` icon is rejected, use `"Microservice"` or `"DefaultBlueprint"`.

## Pause
> "Now agents and skills are distinct, modeled types. Ready for **Step 3 — seeding sample data**?"
