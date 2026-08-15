---
description: Add a new domain agent to an existing Intellara agentic project. Use when the user wants to create, add, or scaffold an agent (finance, hr, legal, risk, retrieval, planning, etc.) inside a project that already exists. Not for starting a new project.
---

# New Agent Skill

When the user wants to add an agent to an existing project:

1. Confirm this is an Intellara agentic project — look for `app/agents/base/`.
   If it isn't there, stop and tell the user to scaffold the project first
   instead of guessing a layout.

2. Ask for the agent's domain name and what it is responsible for (if not given).
   Also ask which existing agent, if any, it should NOT be confused with — two
   agents with overlapping responsibilities make the router unpredictable.

3. Read the real contract before writing anything. Never assume these signatures:
   - `app/agents/base/base_agent.py` — base class, constructor, required methods
   - `app/agents/base/agent_context.py` — the input shape
   - `app/agents/base/agent_result.py` — the output shape
   The code in the project always wins over `./templates/`, which is only a
   reference for the general shape.

4. Create `app/agents/<domain>/<domain>_agent.py` inheriting the base class.
   Match the constructor signature and method names of the existing agents
   exactly — the orchestrator calls every agent the same way.

5. Return the project's result type. Never return a bare dict, string, or
   `None` — `orchestrator/executor.py` relies on one shape from every agent.

6. Wire the agent up. An unregistered agent imports cleanly, passes its own
   tests, and never runs. Check each of these and edit the ones that exist:
   - `orchestrator/router.py` — register so the orchestrator can dispatch to it
   - `core/constants.py` — add the agent identifier / enum member
   - `schemas/` — request and response models, if the API exposes this agent
   - `services/agent_service.py` — if agents are surfaced through the service layer
   - `app/api/v1/agent.py` — only if this agent needs its own endpoint

7. Ask which tools the agent needs and wire them from `app/tools/`. Never
   reimplement something that already exists as a tool.

8. Add a test alongside the project's existing test layout. Cover at least one
   success path and one failure path.

9. Never copy another agent's business logic. Copy its shape, then write the
   new domain's logic from scratch — copied logic carries the other domain's
   constants and assumptions with it.

10. Never hardcode credentials, model names, or endpoints. Read them from
    `core/config.py` or the environment.

11. Before finishing, list every file you created and every file you modified,
    so the user can see that the wiring in step 6 actually happened.
