# Step 3 — Seed Sample Agents & Skills

## Goal
Populate the registry with a realistic spread so the scorecards and dashboard have something to show — including a mix of vetting statuses and success rates so health tiers vary.

## Problem
An empty catalog can't demonstrate differentiation or reliability scoring. We seed dummy (but believable) agents and skills, namespaced with `IKEA -` titles and `ikea_` identifiers.

## What we build
~4 agents and ~4 skills via `upsert_entity`. Spread chosen so health tiers differ:
- A clearly **trusted/Gold** entry (vetted, high success, has docs)
- A **Silver** entry (vetted, good-but-not-great success)
- A **Bronze** entry (in review, moderate success)
- An **unvetted/Basic** entry (no health, to motivate the review flow)

## MCP commands (run for this step only)

### Agents
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_agent",
  "entity": { "identifier": "ikea_agent_pr_reviewer", "title": "IKEA - PR Review Agent",
    "properties": { "purpose": "Autonomously reviews pull requests for style, security and test coverage.", "function": "Reads diffs, runs heuristics + LLM review, posts comments and a verdict.", "category": "coding", "model": "gpt-class", "version": "2.3.0", "lifecycle": "ga", "vetting_status": "vetted", "success_rate": 97, "total_invocations": 5400, "last_evaluated": "2026-06-20T10:00:00Z", "documentation_url": "https://docs.example.ikea/agents/pr-review", "owner": "Developer Experience", "tags": ["code-review", "github"] } } }
```
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_agent",
  "entity": { "identifier": "ikea_agent_incident_triage", "title": "IKEA - Incident Triage Agent",
    "properties": { "purpose": "Triages incoming alerts and proposes likely root cause.", "function": "Correlates alerts, recent deploys and logs; drafts an incident summary.", "category": "devops", "model": "claude-class", "version": "1.1.0", "lifecycle": "beta", "vetting_status": "vetted", "success_rate": 84, "total_invocations": 1200, "last_evaluated": "2026-06-18T09:00:00Z", "documentation_url": "https://docs.example.ikea/agents/triage", "owner": "SRE", "tags": ["incident", "observability"] } } }
```
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_agent",
  "entity": { "identifier": "ikea_agent_data_explainer", "title": "IKEA - Data Explainer Agent",
    "properties": { "purpose": "Answers natural-language questions over the data warehouse.", "function": "Generates SQL, runs it, explains results.", "category": "data", "version": "0.9.0", "lifecycle": "experimental", "vetting_status": "in_review", "success_rate": 68, "total_invocations": 300, "owner": "Data Platform", "tags": ["sql", "analytics"] } } }
```
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_agent",
  "entity": { "identifier": "ikea_agent_release_notes", "title": "IKEA - Release Notes Agent",
    "properties": { "purpose": "Drafts release notes from merged PRs.", "function": "Summarizes changes since last tag.", "category": "productivity", "version": "0.2.0", "lifecycle": "experimental", "vetting_status": "unvetted", "success_rate": 40, "total_invocations": 25, "owner": "Unknown", "tags": ["docs"] } } }
```

### Skills
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_skill",
  "entity": { "identifier": "ikea_skill_format_code", "title": "IKEA - Format Code Skill",
    "properties": { "purpose": "Formats a file or selection to the org style guide.", "function": "Applies prettier/black/gofmt based on language.", "trigger": "/format", "category": "coding", "version": "3.0.1", "lifecycle": "ga", "vetting_status": "vetted", "success_rate": 99, "total_runs": 22000, "last_evaluated": "2026-06-22T08:00:00Z", "documentation_url": "https://docs.example.ikea/skills/format", "owner": "Developer Experience", "tags": ["formatting"] } } }
```
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_skill",
  "entity": { "identifier": "ikea_skill_gen_tests", "title": "IKEA - Generate Tests Skill",
    "properties": { "purpose": "Generates unit tests for a selected function.", "function": "Infers cases and writes a test file.", "trigger": "/gen-tests", "category": "coding", "version": "1.4.0", "lifecycle": "beta", "vetting_status": "vetted", "success_rate": 82, "total_runs": 3100, "last_evaluated": "2026-06-15T08:00:00Z", "documentation_url": "https://docs.example.ikea/skills/gen-tests", "owner": "Quality Eng", "tags": ["testing"] } } }
```
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_skill",
  "entity": { "identifier": "ikea_skill_dep_audit", "title": "IKEA - Dependency Audit Skill",
    "properties": { "purpose": "Audits dependencies for known vulnerabilities.", "function": "Scans manifests and reports CVEs.", "trigger": "/audit-deps", "category": "security", "version": "0.7.0", "lifecycle": "experimental", "vetting_status": "in_review", "success_rate": 71, "total_runs": 540, "owner": "Security", "tags": ["security", "sca"] } } }
```
```
CallMcpTool server: user-port-eu toolName: upsert_entity
  arguments:
{ "blueprintIdentifier": "ikea_registry_skill",
  "entity": { "identifier": "ikea_skill_translate_comments", "title": "IKEA - Translate Comments Skill",
    "properties": { "purpose": "Translates code comments to English.", "function": "Detects language and translates in place.", "trigger": "/translate", "category": "productivity", "version": "0.1.0", "lifecycle": "experimental", "vetting_status": "unvetted", "success_rate": 55, "total_runs": 60, "owner": "Unknown", "tags": ["i18n"] } } }
```

## Checkpoint
- 4 agents and 4 skills exist, with varied `vetting_status` and `success_rate`.

## Verify
```
CallMcpTool server: user-port-eu toolName: list_entities arguments: { "blueprintIdentifier": "ikea_registry_agent" }
CallMcpTool server: user-port-eu toolName: list_entities arguments: { "blueprintIdentifier": "ikea_registry_skill" }
```

## Pause
> "Catalog populated. Ready for **Step 4 — the health scorecards**?"
