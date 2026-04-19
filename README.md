# dev-pipeline — Claude Code Plugin

> A Claude Code plugin that generates a complete, tailored AI agent pipeline for any new software project. From idea to tested code — powered by Claude agents.

**dev-pipeline** reads your project, asks the right questions, and writes five project-specific Claude agents into `.claude/agents/`. Each agent knows your stack, your goals, and your constraints — so you're not starting from scratch every time.

## What It Does

Invoke `/dev-pipeline:dev-pipeline` in any new project. It scans your files, interviews you about scope and requirements, then generates:

| Agent | Role | Output |
|---|---|---|
| `requirements` | Deep requirements interview — covers users, features, data, constraints | `PRD.md` |
| `research` | Researches the best tech stack using context7 MCP + web search | `RESEARCH.md` |
| `mockup` | Builds a visual wireframe in Figma via MCP, loops until you approve | `MOCKUP.md` |
| `codegen` | Orchestrates parallel agent teams to generate the full codebase | Source code |
| `qa` | Security review + comprehensive test suite generation and execution | `QA.md`, `TESTS.md` |

**The skill generates agents — it does not run them.** You then invoke each step using the built-in runner skills, in order.

## Why Use This?

- **Project-specific context baked in** — generated agents know your stack, users, and constraints. Not generic templates.
- **Full lifecycle coverage** — requirements → research → design → code → QA in one coherent pipeline.
- **Works with your existing tools** — integrates with context7, Figma MCP, and Claude Code's experimental agent teams.
- **Saves the setup tax** — instead of manually writing agent instructions for every new project, one command does it.

## Installation

### From marketplace

```
/plugin marketplace add nm616/dev-pipeline
/plugin install dev-pipeline@dev-pipeline
```

### Local install

```bash
git clone https://github.com/nm616/dev-pipeline
claude --plugin-dir ./dev-pipeline
```

## Usage

Open Claude Code in your project directory and run:

```
/dev-pipeline:dev-pipeline
```

Or trigger it naturally — it activates on phrases like:
- *"Set up dev agents for my project"*
- *"Create the agent pipeline"*
- *"Scaffold agents for this repo"*
- *"I'm starting a new project and want agents to help build it"*

After it runs, you'll have:
- `.claude/agents/` — five ready-to-invoke agents
- `PIPELINE.md` — run sequence, prerequisite status, and project summary

## Running the Pipeline

Each step is a runner skill that checks prerequisites automatically and suggests the next step when done:

```
/dev-pipeline:requirements   →  interviews you thoroughly, writes PRD.md
/dev-pipeline:research       →  researches and picks your tech stack, writes RESEARCH.md
/dev-pipeline:mockup         →  creates Figma wireframes, loops until you approve, writes MOCKUP.md
/dev-pipeline:codegen        →  generates the full codebase (parallel agent teams where possible)
/dev-pipeline:qa             →  security review + tests, writes QA.md and TESTS.md
```

## Optional Prerequisites

The generated agents check for these at runtime and include setup instructions if missing — so you don't need them before running `/dev-pipeline`.

| Tool | Used by | Notes |
|---|---|---|
| [context7 MCP](https://github.com/upstash/context7) | Research agent | Live library documentation lookup |
| [Figma MCP](https://github.com/figma/figma-developer-mcp) | Mockup agent | Creates wireframes programmatically |
| Agent Teams flag | Codegen agent | `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` — set automatically |

## How It Works

When you invoke `/dev-pipeline`, it:

1. **Reads your project** — scans files, directories, package.json, README, etc.
2. **Checks your Claude Code settings** — detects whether context7, Figma MCP, and agent teams are configured
3. **Interviews you** — asks targeted questions to fill gaps it couldn't infer
4. **Generates five agents** — each customized with your project's context, constraints, and stack
5. **Writes PIPELINE.md** — documents the sequence, prerequisites, and what each agent does

## Requirements

- [Claude Code](https://claude.ai/code)
- Claude Code plugin system

## License

MIT — see [LICENSE](LICENSE)

---

*Built with Claude Code · Maintained by [@nm616](https://github.com/nm616)*
