# Intellara Plugins for Claude Code

A marketplace of Claude Code plugins for enterprise projects at Intellara.

## Available Plugins

| Plugin | Description |
|---|---|
| `coding-std` | Standard coding practices for Intellara projects |
| `dependency-auditor` | Audits project dependencies for outdated, vulnerable, or risky packages |
| `agent-scaffold` | Scaffolds new agents and tools inside an existing agentic project, with the orchestrator wiring done correctly |
| `dev` | Structured feature development workflow, with specialised agents for codebase exploration, architecture design and quality review, plus terminal flowchart rendering |

## Installation

### 1. Add this marketplace

Inside a Claude Code session, run:

```
/plugin marketplace add vivek760-dev/intellara-plugins-claude
```

(Replace with the local path instead if you're testing before pushing, e.g. `/plugin marketplace add ./intellara-plugins-claude`.)

### 2. Install a plugin

```
/plugin install coding-std@intellara-plugins-claude
```

### 3. Verify installation

```
/plugin
```

This opens the plugin manager where you can see installed plugins and enable/disable them.

## Using Agent-Based Plugins

Some plugins (like `dependency-auditor`) ship an **agent**, not a skill or slash
command. Agents can't be invoked with `/plugin-name` — that will return
`No commands match`. Instead, invoke them with a natural-language request, e.g.:

```
use the dependency-auditor agent to check this project's dependencies
```

Claude Code will route the request to the agent as a subagent task.

## Building Features

`dev` ships two slash commands:

```
/feature-dev add tenant-scoped rate limiting to the agent API
/flowchart the request flow through the auth middleware
```

`/feature-dev` runs a seven-phase workflow: it explores the codebase with parallel
`code-explorer` agents, asks the clarifying questions the request left open, designs
several architectures with competing trade-offs through `code-architect` agents, and
reviews the result with `code-reviewer` agents. It waits for your approval before
writing any code.

`/flowchart` draws a flowchart of code, architecture or a described process as ASCII art
in the terminal. It writes Mermaid source and renders it with a bundled dependency-free
Node script, so there is nothing to install — you get the diagram in your terminal plus
the Mermaid source to paste into GitHub or an IDE preview. `/feature-dev` uses it
automatically to diagram existing control flow and proposed architectures.

The renderer is deliberately limited: top-down layout only, no `subgraph` or styling,
cycles are listed rather than drawn, and roughly fifteen nodes is the practical ceiling
before a diagram outgrows a terminal. See [plugins/dev/README.md](plugins/dev/README.md)
for the full supported Mermaid subset.

## Local Development

To test a plugin without publishing, point Claude Code directly at a plugin folder:

```bash
claude --plugin-dir ./plugins/coding-std
```

## Adding a New Plugin

1. Create a new folder under `plugins/<plugin-name>/`
2. Add a `.claude-plugin/plugin.json` manifest
3. Add a `skills/` folder with one subfolder per skill, each containing a `SKILL.md`
4. Register the plugin in `.claude-plugin/marketplace.json`
