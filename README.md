<<<<<<< HEAD
# IKEA — Trusted Agent & Skill Registry (Cursor onboarding skill)

A guided, step-by-step workshop that builds a registry of trusted, vetted **agents** and **skills** in Port (EU), with a health/reliability scorecard for each. You run it yourself inside Cursor; it guides you one step at a time.

## What you'll build
- Two catalog blueprints: **Trusted Agent** and **Trusted Skill** (so agents and skills are clearly distinct)
- Sample agents & skills to populate the registry
- **Health scorecards** (Bronze/Silver/Gold) driven by vetting status + success rate
- Self-service **Register Agent / Register Skill** actions
- A **Trusted Agent & Skill Registry** dashboard
- A live demo that registers a new agent and shows it scoring Gold

## Prerequisites
- Cursor with agent mode
- Access to your Port organization (EU region)

## One-time setup: connect the Port EU MCP server

This skill talks to Port through the `user-port-eu` MCP server. Confirm it's available in Cursor:

1. Open Cursor **Settings → MCP** (or `Cmd+Shift+P` → "MCP").
2. Ensure a Port server with the ID **`user-port-eu`** is listed and enabled.
3. If it isn't configured, add it (Port EU API). Port's MCP docs: https://docs.port.io (search "MCP server"). You'll authenticate with your Port credentials/region = EU.
4. If a tool call later returns an auth error, re-authenticate the server and retry.

### Quick connectivity test
In a Cursor chat, ask the agent to run:
```
list_blueprints   (server: user-port-eu)
```
If you get a list of blueprints back, you're ready.

## How to run

In Cursor, say:

> **"Start the IKEA agent & skill registry onboarding"**

The skill will preview the 8-step journey and wait for your go-ahead. Then for each step it will:
1. Explain the problem and what you're building
2. Run the Port MCP call(s) for **that step only** (or hand you copy-paste JSON if you prefer the UI)
3. Verify a checkpoint
4. Pause until you're ready for the next step

You can say **"create it for me"** to have it run the current step's MCP calls, or **"let me do it in the UI"** to get the JSON instead.

## Notes
- All identifiers are namespaced (`ikea_registry_*`, `IKEA -` titles) so this is safe to run in a shared org.
- Sample data is dummy data — no external systems or secrets required.
- Nothing is deleted without your explicit approval.

## Files
- `SKILL.md` — the orchestrator the agent follows
- `steps/` — per-step instructions and MCP payloads
- `VALIDATION.md` — end-to-end acceptance checklist
=======
# ikea-agent-skill-registry
>>>>>>> edbd03ad205b1808b03640cc750369f3ef3cfdd7
