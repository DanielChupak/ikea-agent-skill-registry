# Step 7 — Live Demo (Prove the Loop)

## Goal
Demonstrate the full user story end to end: a developer registers a new agent and immediately sees it in the registry — clearly distinct from skills, with its purpose/function readable and a **Gold** health score proving reliability.

## The narrative
> A developer has just hardened their "Secret Scanner" agent. They open the registry, click **Register Agent**, fill in purpose/function, mark it `vetted` with a 98% success rate and a docs link. On submit, it appears in the **Agents** table (not Skills), and its **Agent Health** scorecard lights up **Gold**.

This single action exercises every piece: catalog (agent blueprint), differentiation (lands in Agents, not Skills), purpose/function (captured fields), and health/reliability (Gold tier).

## Run it (MCP)
Run the self-service action with demo inputs:

```
CallMcpTool
  server: user-port-eu
  toolName: run_action
  arguments:
{
  "actionIdentifier": "register_agent",
  "properties": {
    "title": "IKEA - Secret Scanner Agent",
    "purpose": "Scans repositories and PRs for leaked secrets and credentials.",
    "function": "Runs entropy + pattern detection on diffs, blocks merges and notifies owners.",
    "category": "security",
    "model": "claude-class",
    "version": "1.0.0",
    "vetting_status": "vetted",
    "success_rate": 98,
    "documentation_url": "https://docs.example.ikea/agents/secret-scanner",
    "owner": "Security"
  }
}
```

(Or do it from the UI: Port → Self-service → **Register Agent** → fill the form → Execute. Recommended for a live audience.)

## Checkpoint — what to show the developer
1. **It's an agent, not a skill** — the new entry is in the Agents table; the Skills table is unchanged.
2. **Purpose & function are clear** — open the entity; the markdown fields read like a product card.
3. **Health = reliability** — the `Agent Health` scorecard shows **Gold** (vetted + 98% + docs present). Compare against the unvetted "Release Notes Agent" still sitting at **Basic**.
4. **Dashboard updated** — the Agents number chart and the "Agents by vetting status" pie both reflect the new entry.

## Verify
```
CallMcpTool server: user-port-eu toolName: list_entities arguments: { "blueprintIdentifier": "ikea_registry_agent", "identifiers": ["ikea_secret_scanner_agent"] }
```
(Identifier is derived from the title by the action mapping; adjust if your mapping differs.)

## Wrap up
Recap how this maps to the user story:
- *Registry of trusted, vetted agents and skills* → the dashboard + two blueprints
- *Differentiate agents from skills* → separate blueprints + tables; skill-only `trigger` field
- *Understand purpose & function* → dedicated markdown fields on every entry
- *Health scorecard for reliability* → `agent_health` / `skill_health` tiers from custom variables

Point the developer to `VALIDATION.md` to confirm everything, and to the "Optional (Phase 2)" section in `SKILL.md` for automations, an AI concierge, or real-data ingestion.

## Done
> "That's the full loop. Your trusted Agent & Skill Registry is live. Want to tackle a Phase 2 enhancement (review automation, AI concierge, or real-data sync)?"
