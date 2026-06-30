# Step 6 — Registry Dashboard

## Goal
Give developers **one place** to browse the registry, tell agents from skills, read purpose/function, and see each entry's health.

## Problem
The model, data, scores, and actions exist — but they're scattered. The dashboard is the developer-facing front door described in the user story.

## What we build
A dashboard page `ikea_registry` ("Trusted Agent & Skill Registry") with:
1. **Markdown** intro explaining agents vs skills
2. **Number charts**: total agents, total skills
3. **Pie charts**: agents by `vetting_status`, skills by `vetting_status`
4. **Agents table** (`table-entities-explorer`) with health column
5. **Skills table** (`table-entities-explorer`) with health column
6. **Action card** exposing Register Agent / Register Skill

## IMPORTANT: load widget schemas first
Widget JSON varies by Port version. **Before** building, load the schema for each widget type you use, then shape the widget objects to match:

```
CallMcpTool server: user-port-eu toolName: load_widget_schema arguments: { "widgetType": "markdown" }
CallMcpTool server: user-port-eu toolName: load_widget_schema arguments: { "widgetType": "entities-number-chart" }
CallMcpTool server: user-port-eu toolName: load_widget_schema arguments: { "widgetType": "entities-pie-chart" }
CallMcpTool server: user-port-eu toolName: load_widget_schema arguments: { "widgetType": "table-entities-explorer" }
CallMcpTool server: user-port-eu toolName: load_widget_schema arguments: { "widgetType": "action-card-widget" }
```

## MCP command (run for this step only)
Build the page in one call. Layout rows: column sizes must sum to 12; widget count must match layout columns; row height min 400. Adjust each widget's body to whatever `load_widget_schema` returned.

```
CallMcpTool
  server: user-port-eu
  toolName: upsert_dashboard_page
  arguments:
{
  "identifier": "ikea_registry",
  "title": "Trusted Agent & Skill Registry",
  "icon": "AI",
  "visibility": "org",
  "widgets": [
    { "id": "intro", "type": "markdown", "markdown": "# Trusted Agent & Skill Registry\n\nBrowse vetted **agents** (autonomous) and **skills** (invoked on demand). Each entry shows its purpose, function and a **health score** (reliability) based on vetting status and success rate.\n\n- **Agent** = runs autonomously toward a goal\n- **Skill** = a single capability you trigger (e.g. `/format`)" },
    { "id": "total_agents", "type": "entities-number-chart", "title": "Agents", "icon": "AI", "description": "Total agents", "blueprint": "ikea_registry_agent", "function": "count", "calculationBy": "entities" },
    { "id": "total_skills", "type": "entities-number-chart", "title": "Skills", "icon": "Book", "description": "Total skills", "blueprint": "ikea_registry_skill", "function": "count", "calculationBy": "entities" },
    { "id": "agents_by_vetting", "type": "entities-pie-chart", "title": "Agents by vetting status", "blueprint": "ikea_registry_agent", "property": "property#vetting_status" },
    { "id": "skills_by_vetting", "type": "entities-pie-chart", "title": "Skills by vetting status", "blueprint": "ikea_registry_skill", "property": "property#vetting_status" },
    { "id": "agents_table", "type": "table-entities-explorer", "title": "Agents", "icon": "AI", "blueprint": "ikea_registry_agent", "dataset": { "combinator": "and", "rules": [ { "property": "$blueprint", "operator": "=", "value": "ikea_registry_agent" } ] } },
    { "id": "skills_table", "type": "table-entities-explorer", "title": "Skills", "icon": "Book", "blueprint": "ikea_registry_skill", "dataset": { "combinator": "and", "rules": [ { "property": "$blueprint", "operator": "=", "value": "ikea_registry_skill" } ] } },
    { "id": "register_actions", "type": "action-card-widget", "title": "Register", "actions": [ { "actionIdentifier": "register_agent" }, { "actionIdentifier": "register_skill" } ] }
  ],
  "layout": [
    { "height": 400, "columns": [ { "id": "intro", "size": 12 } ] },
    { "height": 400, "columns": [ { "id": "total_agents", "size": 3 }, { "id": "total_skills", "size": 3 }, { "id": "agents_by_vetting", "size": 3 }, { "id": "skills_by_vetting", "size": 3 } ] },
    { "height": 500, "columns": [ { "id": "agents_table", "size": 12 } ] },
    { "height": 500, "columns": [ { "id": "skills_table", "size": 12 } ] },
    { "height": 400, "columns": [ { "id": "register_actions", "size": 12 } ] }
  ]
}
```

## If a widget is rejected
The exact keys (`property`, `dataset`, `function`, etc.) differ by version. Re-shape that one widget from its `load_widget_schema` output and re-run. You can also add widgets individually with `upsert_widget` (`pageIdentifier: "ikea_registry"`).

## Make health visible in the tables
After the page exists, configure the Agents/Skills table columns in the UI (or via the table widget schema) to show the `Agent Health` / `Skill Health` scorecard column, plus `vetting_status`, `success_rate`, `category`, and `owner`.

## Checkpoint
- Page "Trusted Agent & Skill Registry" renders with both tables, the charts, and the register action card.

## Verify
Open Port → the new page in the sidebar, or:
```
CallMcpTool server: user-port-eu toolName: get_page arguments: { "identifier": "ikea_registry" }
```

## Pause
> "The registry is live and browsable. Ready for **Step 7 — the live demo**?"
