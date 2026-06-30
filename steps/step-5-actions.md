# Step 5 — Self-Service Register Actions

## Goal
Let developers add new agents and skills to the registry themselves, with the right fields captured so health scoring works immediately.

## Problem
A registry that only an admin can edit goes stale. We give developers two self-service actions that create a registry entry directly — no backend or secrets, using Port's native `UPSERT_ENTITY` invocation.

## What we build
- `register_agent` — CREATE on `ikea_registry_agent`
- `register_skill` — CREATE on `ikea_registry_skill`

## MCP commands (run for this step only)

### Register Agent
```
CallMcpTool
  server: user-port-eu
  toolName: upsert_action
  arguments:
{
  "action": {
    "identifier": "register_agent",
    "title": "Register Agent",
    "icon": "AI",
    "description": "Add a new agent to the trusted registry.",
    "trigger": {
      "type": "self-service",
      "operation": "CREATE",
      "blueprintIdentifier": "ikea_registry_agent",
      "userInputs": {
        "properties": {
          "title": { "type": "string", "title": "Agent name" },
          "purpose": { "type": "string", "format": "markdown", "title": "Purpose" },
          "function": { "type": "string", "format": "markdown", "title": "Function" },
          "category": { "type": "string", "title": "Category", "enum": ["coding", "data", "devops", "security", "productivity", "other"] },
          "model": { "type": "string", "title": "Model / Runtime" },
          "version": { "type": "string", "title": "Version" },
          "vetting_status": { "type": "string", "title": "Vetting status", "enum": ["unvetted", "in_review", "vetted", "deprecated"], "default": "unvetted" },
          "success_rate": { "type": "number", "title": "Success rate (%)" },
          "documentation_url": { "type": "string", "format": "url", "title": "Documentation" },
          "owner": { "type": "string", "title": "Owner" }
        },
        "required": ["title", "purpose", "vetting_status"],
        "order": ["title", "purpose", "function", "category", "model", "version", "vetting_status", "success_rate", "documentation_url", "owner"]
      }
    },
    "invocationMethod": {
      "type": "UPSERT_ENTITY",
      "blueprintIdentifier": "ikea_registry_agent",
      "mapping": {
        "identifier": "{{ .inputs.title | gsub \"[^a-zA-Z0-9]+\";\"_\" | ascii_downcase }}",
        "title": "{{ .inputs.title }}",
        "properties": {
          "purpose": "{{ .inputs.purpose }}",
          "function": "{{ .inputs.function }}",
          "category": "{{ .inputs.category }}",
          "model": "{{ .inputs.model }}",
          "version": "{{ .inputs.version }}",
          "vetting_status": "{{ .inputs.vetting_status }}",
          "success_rate": "{{ .inputs.success_rate }}",
          "documentation_url": "{{ .inputs.documentation_url }}",
          "owner": "{{ .inputs.owner }}",
          "last_evaluated": "{{ .trigger.at }}"
        }
      }
    }
  }
}
```

### Register Skill
```
CallMcpTool
  server: user-port-eu
  toolName: upsert_action
  arguments:
{
  "action": {
    "identifier": "register_skill",
    "title": "Register Skill",
    "icon": "Book",
    "description": "Add a new skill to the trusted registry.",
    "trigger": {
      "type": "self-service",
      "operation": "CREATE",
      "blueprintIdentifier": "ikea_registry_skill",
      "userInputs": {
        "properties": {
          "title": { "type": "string", "title": "Skill name" },
          "purpose": { "type": "string", "format": "markdown", "title": "Purpose" },
          "function": { "type": "string", "format": "markdown", "title": "Function" },
          "trigger_input": { "type": "string", "title": "Trigger / invocation" },
          "category": { "type": "string", "title": "Category", "enum": ["coding", "data", "devops", "security", "productivity", "other"] },
          "version": { "type": "string", "title": "Version" },
          "vetting_status": { "type": "string", "title": "Vetting status", "enum": ["unvetted", "in_review", "vetted", "deprecated"], "default": "unvetted" },
          "success_rate": { "type": "number", "title": "Success rate (%)" },
          "documentation_url": { "type": "string", "format": "url", "title": "Documentation" },
          "owner": { "type": "string", "title": "Owner" }
        },
        "required": ["title", "purpose", "vetting_status"],
        "order": ["title", "purpose", "function", "trigger_input", "category", "version", "vetting_status", "success_rate", "documentation_url", "owner"]
      }
    },
    "invocationMethod": {
      "type": "UPSERT_ENTITY",
      "blueprintIdentifier": "ikea_registry_skill",
      "mapping": {
        "identifier": "{{ .inputs.title | gsub \"[^a-zA-Z0-9]+\";\"_\" | ascii_downcase }}",
        "title": "{{ .inputs.title }}",
        "properties": {
          "purpose": "{{ .inputs.purpose }}",
          "function": "{{ .inputs.function }}",
          "trigger": "{{ .inputs.trigger_input }}",
          "category": "{{ .inputs.category }}",
          "version": "{{ .inputs.version }}",
          "vetting_status": "{{ .inputs.vetting_status }}",
          "success_rate": "{{ .inputs.success_rate }}",
          "documentation_url": "{{ .inputs.documentation_url }}",
          "owner": "{{ .inputs.owner }}",
          "last_evaluated": "{{ .trigger.at }}"
        }
      }
    }
  }
}
```

> Note: the skill's user input is named `trigger_input` (not `trigger`) to avoid clashing with the action's own `trigger` block; the mapping writes it into the `trigger` property.

## Checkpoint
- `register_agent` and `register_skill` appear in Port → Self-service.

## Verify
```
CallMcpTool server: user-port-eu toolName: list_actions arguments: {}
```

## Pause
> "Developers can now self-register agents and skills. Ready for **Step 6 — the registry dashboard**?"
