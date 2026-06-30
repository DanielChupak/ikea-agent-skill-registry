# Step 4 — Health Scorecards

## Goal
Turn raw fields into a **reliability signal** developers can read at a glance: a Bronze / Silver / Gold health tier for every agent and skill.

## Problem
`vetting_status` and `success_rate` are useful, but a developer scanning the registry wants one answer: *can I trust this?* Scorecards collapse the custom variables into a tier.

## Custom variables → tiers
Levels are **Basic → Bronze → Silver → Gold** (Basic is the floor; rules cannot target Basic).

| Tier | Rule (all conditions AND'd) |
|------|------------------------------|
| Bronze | `vetting_status != unvetted` **and** `success_rate >= 50` |
| Silver | `vetting_status = vetted` **and** `success_rate >= 80` |
| Gold | `vetting_status = vetted` **and** `success_rate >= 95` **and** `documentation_url` is not empty |

> Boolean/number reminder: number conditions use numeric `value`; string conditions use string `value`. Never use 1/0 for booleans.

## MCP commands (run for this step only)

### Agent health
```
CallMcpTool
  server: user-port-eu
  toolName: upsert_scorecard
  arguments:
{
  "blueprintIdentifier": "ikea_registry_agent",
  "scorecard": {
    "identifier": "agent_health",
    "title": "Agent Health",
    "rules": [
      { "identifier": "reviewed_and_working", "title": "Reviewed & working", "level": "Bronze",
        "query": { "combinator": "and", "conditions": [
          { "property": "vetting_status", "operator": "!=", "value": "unvetted" },
          { "property": "success_rate", "operator": ">=", "value": 50 } ] } },
      { "identifier": "vetted_reliable", "title": "Vetted & reliable", "level": "Silver",
        "query": { "combinator": "and", "conditions": [
          { "property": "vetting_status", "operator": "=", "value": "vetted" },
          { "property": "success_rate", "operator": ">=", "value": 80 } ] } },
      { "identifier": "production_trusted", "title": "Production trusted", "level": "Gold",
        "query": { "combinator": "and", "conditions": [
          { "property": "vetting_status", "operator": "=", "value": "vetted" },
          { "property": "success_rate", "operator": ">=", "value": 95 },
          { "property": "documentation_url", "operator": "isNotEmpty" } ] } }
    ]
  }
}
```

### Skill health
```
CallMcpTool
  server: user-port-eu
  toolName: upsert_scorecard
  arguments:
{
  "blueprintIdentifier": "ikea_registry_skill",
  "scorecard": {
    "identifier": "skill_health",
    "title": "Skill Health",
    "rules": [
      { "identifier": "reviewed_and_working", "title": "Reviewed & working", "level": "Bronze",
        "query": { "combinator": "and", "conditions": [
          { "property": "vetting_status", "operator": "!=", "value": "unvetted" },
          { "property": "success_rate", "operator": ">=", "value": 50 } ] } },
      { "identifier": "vetted_reliable", "title": "Vetted & reliable", "level": "Silver",
        "query": { "combinator": "and", "conditions": [
          { "property": "vetting_status", "operator": "=", "value": "vetted" },
          { "property": "success_rate", "operator": ">=", "value": 80 } ] } },
      { "identifier": "production_trusted", "title": "Production trusted", "level": "Gold",
        "query": { "combinator": "and", "conditions": [
          { "property": "vetting_status", "operator": "=", "value": "vetted" },
          { "property": "success_rate", "operator": ">=", "value": 95 },
          { "property": "documentation_url", "operator": "isNotEmpty" } ] } }
    ]
  }
}
```

## Checkpoint
With the Step 3 seed data, expect roughly:
- PR Review Agent → **Gold** (vetted, 97%, docs)
- Incident Triage Agent → **Silver** (vetted, 84%)
- Data Explainer Agent → **Bronze** (in_review, 68%)
- Release Notes Agent → **Basic** (unvetted)
- Format Code Skill → **Gold**; Generate Tests → **Silver**; Dependency Audit → **Bronze**; Translate Comments → **Basic**

## Verify
```
CallMcpTool server: user-port-eu toolName: list_scorecards arguments: {}
```
Then in Port, open an entity to see its health level, or use `list_entities` with a `scorecard` filter rule.

## Extending the health model
Add more custom variables later by adding properties (e.g. `last_evaluated` freshness, `test_coverage`, `security_review_passed`) and new rule conditions. The structure above is the template.

## Pause
> "Reliability scoring is live. Ready for **Step 5 — self-service register actions**?"
