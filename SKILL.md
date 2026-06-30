---
name: ikea-agent-skill-registry
description: >-
  Guided Port onboarding that builds a registry of trusted, vetted AI agents and
  skills for developers. Differentiates agents from skills, captures purpose and
  function, and computes a health/reliability scorecard from custom variables
  (vetting status + success rate). Run this step by step in Cursor with the Port
  EU MCP server connected. Use when you want to stand up an internal Agent & Skill
  catalog with reliability scoring.
---

# IKEA — Trusted Agent & Skill Registry

An interactive, step-by-step workshop that builds a **registry of trusted and vetted agents and skills** in Port, so developers can:

- **Browse** a single catalog of agents and skills
- **Differentiate** an agent from a skill, and understand each one's **purpose** and **function**
- **Judge reliability** at a glance via a **health scorecard** driven by custom variables (vetting status + success rate)

You (the developer) drive this. I guide one step at a time, build with you using the Port MCP, checkpoint, and pause.

---

## Behavior — CRITICAL

Read and obey these rules for the entire session. They override any instinct to be "helpful" by doing everything at once.

1. **One step at a time.** Work only on the **current** step. Never run MCP calls for future steps. Never batch Steps 1–N in a single turn.
2. **Explain → build together → checkpoint → pause.** For each step: state the problem, say what we're building, run the MCP call(s) for *this step only*, verify the checkpoint, then **stop and wait** for the user to say they're ready for the next step.
3. **On activation, do NOT build anything.** Present the journey overview + step count, then ask: *"Ready for Step 0?"* Wait for a "let's go" before doing anything.
4. **Escape hatch.** If the user says "create it for me" / "just do it", run the MCP calls for the **current step only**, then pause as normal.
5. **Manual fallback.** If the user prefers the UI, give them the copy-paste JSON from the step file instead of calling MCP.
6. **Data.** Use dummy/sample data, namespaced with the `ikea_registry_` / `IKEA -` convention. Do not pre-create demo data before the demo step (Step 7).
7. **Never delete** anything in the org without explicit user approval.
8. **Inspect before create.** If a blueprint/entity may already exist, call the relevant `list_*` tool first and adapt.
9. **MCP server is `user-port-eu`.** Always read the tool schema from the MCP filesystem before a call you're unsure about.
10. **Tone:** professional, hands-on, concise. Guide; don't dump walls of JSON at activation — the JSON lives in the step files and is shown only when its step is active.

---

## Region & MCP

| Setting | Value |
|---------|-------|
| Port region | EU |
| MCP server ID | `user-port-eu` |

Key tools used: `list_blueprints`, `upsert_blueprint`, `upsert_entity`, `upsert_scorecard`, `upsert_action`, `upsert_dashboard_page`, `load_widget_schema`, `run_action`.

---

## Activation Script

When the user starts this skill (e.g. *"Start the IKEA agent & skill registry onboarding"*), respond with the welcome below, then **stop**:

> Welcome — we're going to build a **Trusted Agent & Skill Registry** in your Port (EU) org, one step at a time.
>
> By the end, a developer will be able to open one dashboard, tell agents from skills, read each one's purpose and function, and see a **health score** (reliability) for every entry.
>
> Here's the journey (8 steps):
>
> | Step | What we build | Why it matters |
> |------|---------------|----------------|
> | 0 | Orientation & connectivity check | Confirm Port EU MCP is reachable |
> | 1 | `Trusted Agent` blueprint | Model agents + their purpose/function |
> | 2 | `Trusted Skill` blueprint | Model skills, distinct from agents |
> | 3 | Seed sample agents & skills | Give the registry something to show |
> | 4 | Health scorecards | Turn vetting status + success rate into a reliability tier |
> | 5 | Self-service register actions | Let developers add new agents/skills |
> | 6 | Registry dashboard | One place to browse and compare |
> | 7 | Live demo | Register a new agent → watch its health compute |
>
> We'll do **one step at a time** — I'll build with you, we'll verify, then move on when you're ready.
>
> **Ready for Step 0?**

Do not proceed past this message until the user confirms.

---

## Step Orchestration

For each step: read the matching file in `steps/` for full MCP payloads and verification URLs. Below is the inline contract for what to do in each step. **Only act on the current step.**

### Step 0 — Orientation & connectivity → `steps/step-0-orientation.md`
- **Problem:** We need to confirm the Port EU MCP is connected before building.
- **Build together:** Run `list_blueprints` (summary) to confirm connectivity and check that `ikea_registry_agent` / `ikea_registry_skill` don't already exist.
- **Checkpoint:** Tool returns a blueprint list; no naming collisions (or we agree to reuse/extend).
- **Pause:** "Connectivity confirmed. Ready for Step 1 — the Trusted Agent blueprint?"

### Step 1 — Trusted Agent blueprint → `steps/step-1-agent-blueprint.md`
- **Problem:** Developers can't browse agents that aren't modeled. We need a blueprint that captures an agent's **purpose**, **function**, and the variables that feed health.
- **Build together:** `upsert_blueprint` for `ikea_registry_agent`.
- **Checkpoint:** Blueprint exists with `vetting_status` and `success_rate` properties.
- **Pause:** "Agent model is in. Ready for Step 2 — the Trusted Skill blueprint?"

### Step 2 — Trusted Skill blueprint → `steps/step-2-skill-blueprint.md`
- **Problem:** A skill is **not** an agent — developers must be able to tell them apart. We model skills in a parallel blueprint.
- **Build together:** `upsert_blueprint` for `ikea_registry_skill`.
- **Checkpoint:** Both blueprints exist side by side.
- **Pause:** "Now we can differentiate agents and skills. Ready for Step 3 — sample data?"

### Step 3 — Seed sample agents & skills → `steps/step-3-seed-entities.md`
- **Problem:** An empty registry proves nothing. Seed a spread of vetted/unvetted, high/low reliability entries.
- **Build together:** `upsert_entity` for ~4 agents and ~4 skills (dummy, `IKEA -` titles).
- **Checkpoint:** Entities visible under each blueprint.
- **Pause:** "Catalog has data. Ready for Step 4 — the health scorecards?"

### Step 4 — Health scorecards → `steps/step-4-scorecards.md`
- **Problem:** Developers need reliability at a glance, not raw fields. Convert vetting status + success rate into Bronze/Silver/Gold health tiers.
- **Build together:** `upsert_scorecard` `agent_health` (on agent BP) and `skill_health` (on skill BP).
- **Checkpoint:** Seeded entities now show a health level.
- **Pause:** "Health scoring is live. Ready for Step 5 — self-service register actions?"

### Step 5 — Register actions → `steps/step-5-actions.md`
- **Problem:** The registry must grow without an admin. Give developers self-service actions to register a new agent or skill.
- **Build together:** `upsert_action` `register_agent` and `register_skill` (CREATE → UPSERT_ENTITY, no backend needed).
- **Checkpoint:** Both actions appear in the self-service catalog.
- **Pause:** "Developers can now self-register. Ready for Step 6 — the dashboard?"

### Step 6 — Registry dashboard → `steps/step-6-dashboard.md`
- **Problem:** Everything we built needs one home. Build the browsing/compare experience.
- **Build together:** `upsert_dashboard_page` `ikea_registry` with markdown intro, number/pie charts, agent & skill tables (health column), and the register action card.
- **Checkpoint:** Page renders with both tables and health visible.
- **Pause:** "The registry is live. Ready for Step 7 — the demo?"

### Step 7 — Live demo → `steps/step-7-demo.md`
- **Problem:** Prove the whole loop end to end.
- **Build together:** `run_action` `register_agent` with a new vetted, high-success agent (or do it from the UI). Watch it land in the Agents table, distinct from skills, with its purpose/function readable and a **Gold** health tier.
- **Checkpoint:** New agent appears + scores Gold; dashboard counts update.
- **Close:** Recap how this satisfies the user story and point to `VALIDATION.md`.

---

## Identifiers (namespaced for a shared org)

| Artifact | Identifier |
|----------|------------|
| Agent blueprint | `ikea_registry_agent` |
| Skill blueprint | `ikea_registry_skill` |
| Agent scorecard | `agent_health` (on `ikea_registry_agent`) |
| Skill scorecard | `skill_health` (on `ikea_registry_skill`) |
| Register agent action | `register_agent` |
| Register skill action | `register_skill` |
| Dashboard page | `ikea_registry` |

---

## Custom health variables (this build)

The reliability score is driven by the variables chosen in discovery:

- **Vetting status** — `unvetted` → `in_review` → `vetted` (plus `deprecated`)
- **Success rate (%)** — observed pass/success rate of the agent or skill

Supporting signal used only for the top tier: **documentation present** (so a "trusted" entry also explains its purpose/function). The scorecard rules are easy to extend with more variables later — see `steps/step-4-scorecards.md`.

---

## Safety

- All identifiers are namespaced (`ikea_registry_*`, `IKEA -` titles) to avoid collisions in a shared org.
- No deletes without explicit user approval.
- Actions use Port's native `UPSERT_ENTITY` invocation, so no external backend or secrets are required.

## Optional (Phase 2)

Not built by default; offer if the user asks:
- **Automation:** flag newly registered `unvetted` agents/skills for review (`ENTITY_CREATED` trigger).
- **AI agent:** a "registry concierge" that recommends a trusted agent/skill for a task.
- **Real data:** sync agents/skills from a GitHub repo or internal catalog instead of dummy entities.

See `VALIDATION.md` for the end-to-end acceptance checklist.
