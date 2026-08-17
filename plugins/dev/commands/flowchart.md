---
description: Draw a flowchart of code, architecture, or a described process directly in the terminal
argument-hint: What to diagram (e.g. "the auth flow" or a path)
---

# Flowchart

Produce a flowchart of what the user asked for and render it as ASCII art in the terminal.

Target: $ARGUMENTS

## How this works

You write Mermaid flowchart source, then render it with the bundled renderer. The
renderer has no dependencies — it is plain Node with nothing to install.

## Steps

1. **Understand the subject.** If the target is code, read it first — trace the real
   control flow rather than guessing from names. Launch `code-explorer` agents if the
   flow spans many files. If the target is a described process, use what the user said.

2. **Ask before drawing if the scope is ambiguous.** A flowchart of "the backend" is
   useless; a flowchart of "a request through the auth middleware" is not. Confirm the
   entry point and the level of detail when it is unclear.

3. **Write the Mermaid source** to a `.mmd` file (use a temp path unless the user wants
   it kept). Keep it to the supported subset below.

4. **Render it:**

   ```bash
   node "${CLAUDE_PLUGIN_ROOT}/scripts/flowchart.js" diagram.mmd
   ```

   Add `--ascii` for terminals without box-drawing character support:

   ```bash
   node "${CLAUDE_PLUGIN_ROOT}/scripts/flowchart.js" diagram.mmd --ascii
   ```

5. **Show the rendered output** in your reply inside a plain fenced code block, so the
   alignment is preserved. Below it, offer the Mermaid source in a ```mermaid block for
   the user to paste into GitHub, an Artifact, or an IDE preview.

6. **Check the render.** Read the output before showing it. If boxes collide, arrows
   look tangled, or the diagram is far too wide for a terminal, simplify: collapse
   detail into fewer nodes, shorten labels, or split into two diagrams. A cluttered
   flowchart is worse than a paragraph of prose.

## Supported Mermaid subset

The renderer handles a deliberate subset. Stay inside it:

| Feature | Syntax |
|---|---|
| Header | `flowchart TD` (also `TB`; `LR`/`RL`/`BT` render top-down) |
| Rectangle | `A[Label]` |
| Rounded | `B(Label)` |
| Circle | `C((Label))` |
| Decision | `D{Label?}` |
| Subroutine | `E[[Label]]` |
| Datastore | `F[(Label)]` |
| Edge | `A --> B`, `A --- B`, `A -.-> B`, `A ==> B` |
| Edge label | `A -->|yes| B` or `A -- yes --> B` |
| Chain | `A --> B --> C` |
| Line break in a label | `A[First<br/>Second]` |
| Comment | `%% ignored` |

**Not supported** — `subgraph`, `style`, `classDef`, `click`, `linkStyle`. These lines
are skipped with a warning rather than failing the render, but do not write them.

Cycles are fine. Edges that loop backward cannot be routed downward, so they are listed
under a `loops back:` note beneath the diagram.

## Style guidance

- **Layout is top-down.** Write the flow so it reads downward; the renderer only does
  vertical layering.
- **Keep labels short.** Every character widens a box, and width is the main driver of
  an unreadable terminal diagram. Aim for under 20 characters, and use `<br/>` rather
  than one long line.
- **Ten to fifteen nodes is the practical ceiling.** Past that, terminal width runs out.
  Split into multiple diagrams by phase or subsystem.
- **Match shape to meaning:** rectangles for steps, `{}` for branches, `[( )]` for
  datastores, `(( ))` for start/end.
- **Prefer few crossings.** Order nodes so edges mostly run straight down; the renderer
  reduces crossings but cannot eliminate them.
