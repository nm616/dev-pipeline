# dev-pipeline

A Claude Code plugin that generates a tailored suite of development agent files for any new project. Point it at a fresh repo, answer a few questions, and it writes five ready-to-use agents into `.claude/agents/` — each pre-loaded with your project's context.

## What It Does

When invoked, the `dev-pipeline` skill:

1. Reads your project files and asks targeted questions to understand what you're building
2. Generates five customized agent files in `.claude/agents/`
3. Writes a `PIPELINE.md` to the project root with the run sequence and prerequisite status

**The skill generates agents — it does not run them.** You invoke each agent yourself when ready.

| Agent | What it does | Output |
|---|---|---|
| `requirements.md` | Deep requirements interview | `PRD.md` |
| `research.md` | Tech stack research via context7 + web | `RESEARCH.md` |
| `mockup.md` | Figma wireframe with approval loop | `MOCKUP.md` |
| `codegen.md` | Code generation (supports agent teams) | Source code |
| `qa.md` | Security review + test suite | `QA.md`, `TESTS.md` |

## Installation

### From GitHub (recommended)

```bash
claude plugin install https://github.com/nigelmorford/dev-pipeline
```

### Manual

```bash
git clone https://github.com/nigelmorford/dev-pipeline
claude plugin install ./dev-pipeline
```

### Enable the plugin

After installing, enable it in Claude Code:

```bash
claude plugin enable dev-pipeline
```

## Usage

Navigate to your project directory and invoke the skill:

```
/dev-pipeline
```

Or just describe what you want — the skill triggers automatically on phrases like:
- "Set up dev agents for my project"
- "Scaffold agents for this repo"
- "Create the dev pipeline agents"
- "I'm starting a new project and want agents to help"

## Running the Pipeline

After the skill runs, invoke each agent in order:

```
/requirements   →  produces PRD.md
/research       →  produces RESEARCH.md
/mockup         →  produces MOCKUP.md (loops until you approve)
/codegen        →  generates the codebase
/qa             →  reviews and tests everything
```

See `PIPELINE.md` in your project root for project-specific notes and prerequisite status.

## Prerequisites

The generated agents have optional dependencies. The skill checks your configuration at runtime and embeds setup instructions directly into any agent that needs them — so you'll always know what to do before invoking that step.

| Tool | Used by | How to get it |
|---|---|---|
| context7 MCP | Research agent | Enable `context7@claude-plugins-official` in Claude Code settings |
| Figma MCP | Mockup agent | Requires a Figma account + npm install — agent includes setup steps |
| Agent Teams flag | Codegen agent | Agent sets `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` automatically if missing |

## License

MIT
