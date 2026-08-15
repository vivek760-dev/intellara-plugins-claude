---
description: Add a new tool that agents can call, inside an existing Intellara agentic project. Use when the user wants to create, add, or register a tool — database, search, rag, email, filesystem, browser, calculator, or a custom one. Not for starting a new project.
---

# New Tool Skill

When the user wants to add a tool to an existing project:

1. Confirm this is an Intellara agentic project — look for `app/tools/base/`.
   If it isn't there, stop and tell the user to scaffold the project first.

2. Ask what the tool does, what inputs it takes, and what it returns
   (if not given).

3. Place it in the right category folder under `app/tools/`:
   `database/`, `search/`, `rag/`, `email/`, `filesystem/`, `browser/`,
   `calculator/`. Use `custom/` only when it genuinely fits none of these, and
   say out loud why — `custom/` becomes a dumping ground otherwise.

4. Read `app/tools/base/` for the real base class and registration pattern
   before writing anything. `./templates/` is only a reference for the shape.

5. Write the tool definition as if it were a prompt, because it is — the model
   reads it at runtime to decide whether to call this tool:
   - **name**: `verb_noun`, lowercase, unambiguous against every existing tool
   - **description**: what it does, *when to call it*, and *when not to*.
     A vague description means the tool is silently never called — no error,
     no crash, just an agent that quietly underperforms.
   - **parameters**: explicit JSON schema. Every field gets a type, a
     description, and a required/optional marker. The model cannot guess.

6. Implement the tool:
   - Return a structured failure result. Never let an exception escape into the
     agent loop — a raised error can send the agent into a retry loop it cannot
     see the cause of.
   - Put a timeout on every network, database, or browser call.
   - Never hardcode credentials or endpoints — read them from `core/config.py`
     or the environment.

7. Register the tool in the project's tool registry so agents can actually
   reach it. An unregistered tool is dead code.

8. Give the tool the narrowest access it needs to do its job. A search tool
   does not need write credentials.

9. Add a test covering both the success path and the failure path. The failure
   path matters more here — that's the one agents actually trip over.

10. Before finishing, list every file you created and every file you modified,
    and show the final tool description so the user can judge whether a model
    would call it correctly.
