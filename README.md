# Intellara Plugins for Claude Code

A marketplace of Claude Code plugins for enterprise projects at Intellara.

## Available Plugins

| Plugin | Description |
|---|---|
| `coding-std` | Standard coding practices for Intellara projects |
| `dependency-auditor` | Audits project dependencies for outdated, vulnerable, or risky packages |

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
