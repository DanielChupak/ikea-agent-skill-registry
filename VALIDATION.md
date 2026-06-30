# Validation — Trusted Agent & Skill Registry

End-to-end checklist. Tick each item after running the corresponding step. The user story is satisfied when all Core items pass.

## User story coverage

| User story need | Satisfied by | Check |
|-----------------|--------------|-------|
| Registry of trusted, vetted agents & skills | Dashboard + two blueprints + seed data | ☐ |
| Differentiate agents from skills | Separate `Trusted Agent` / `Trusted Skill` blueprints + separate tables + skill-only `trigger` | ☐ |
| Understand purpose & function | `purpose` + `function` markdown fields on each entry | ☐ |
| Health scorecard for reliability | `agent_health` / `skill_health` Bronze/Silver/Gold tiers | ☐ |
| Reliability from custom variables | Rules use `vetting_status` + `success_rate` (+ docs for Gold) | ☐ |

## Core (built by this skill)

### Step 0 — Connectivity
- ☐ `list_blueprints` returns a list from `user-port-eu`
- ☐ No collision on `ikea_registry_agent` / `ikea_registry_skill` (or reuse agreed)

### Step 1 — Agent blueprint
- ☐ `ikea_registry_agent` exists
- ☐ Has `purpose`, `function`, `vetting_status` (enum), `success_rate` (number), `documentation_url`

### Step 2 — Skill blueprint
- ☐ `ikea_registry_skill` exists
- ☐ Has a `trigger` property (distinguishes skills from agents)
- ☐ Both blueprints visible side by side

### Step 3 — Seed data
- ☐ ~4 agents exist with varied `vetting_status` / `success_rate`
- ☐ ~4 skills exist with varied `vetting_status` / `success_rate`

### Step 4 — Scorecards
- ☐ `agent_health` exists on `ikea_registry_agent` with Bronze/Silver/Gold rules
- ☐ `skill_health` exists on `ikea_registry_skill` with Bronze/Silver/Gold rules
- ☐ Seeded entities show varied tiers (Gold / Silver / Bronze / Basic)
- ☐ No rule targets the `Basic` level (would 409)

### Step 5 — Actions
- ☐ `register_agent` appears in Self-service
- ☐ `register_skill` appears in Self-service
- ☐ Both use `UPSERT_ENTITY` (no backend required)

### Step 6 — Dashboard
- ☐ Page `ikea_registry` renders
- ☐ Agents table and Skills table both present (differentiation visible)
- ☐ Number + pie charts render
- ☐ Health scorecard column visible in the tables
- ☐ Register action card present

### Step 7 — Demo
- ☐ Running `register_agent` creates a new agent entity
- ☐ New entity lands in the Agents table (not Skills)
- ☐ Its purpose/function are readable
- ☐ Its `Agent Health` scorecard = **Gold**
- ☐ Dashboard counts/pie update

## Build-order dependencies
1. Blueprints (Steps 1–2) before entities (Step 3)
2. Blueprints before scorecards (Step 4)
3. Blueprints + actions (Step 5) before dashboard action card (Step 6)
4. Everything before the demo (Step 7)

## Safety
- ☐ No resources deleted without explicit approval
- ☐ All new identifiers namespaced (`ikea_registry_*`, `IKEA -`)

## Optional (Phase 2 — not built by default)
- ☐ Automation: flag newly registered `unvetted` entries for review (`ENTITY_CREATED` trigger)
- ☐ AI agent: "registry concierge" recommending a trusted agent/skill for a task
- ☐ Real data: sync agents/skills from GitHub or an internal catalog instead of dummy entities
- ☐ Extra health variables: freshness (`last_evaluated`), test coverage, security review
