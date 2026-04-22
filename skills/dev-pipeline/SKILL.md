---
description: Generates a tailored suite of development agent files for a new project. Scans the project, asks targeted questions, then writes five ready-to-use agents into .claude/agents/ covering requirements, research, mockup, code generation, and QA. Use when the user says "set up agents for my project", "create dev pipeline agents", "scaffold agents", "generate agents for this project", "build out the agents", "I'm starting a new project and want agents to help", or describes a new project they want to build.
disable-model-invocation: true
---

# Dev Pipeline — Agent Generator

This skill scans the current project and generates five tailored agent files in `.claude/agents/`. Each agent is pre-loaded with project-specific context. The skill generates agents — it does not run them.

## Current project state

```!
ls -la 2>/dev/null
```

---

## Phase 1: Understand the Project

The project snapshot and settings above give you a starting point. Extract everything you can from existing files before asking anything.

Read any README, package.json, pyproject.toml, Cargo.toml, or planning docs present. Then ask only about genuine gaps — group questions logically and wait for answers between groups:

**Project identity** (skip if clear from files)
- What does this project do and what problem does it solve?
- Who will use it — and are they technical or non-technical?
- Is this a prototype, MVP, or production system?

**Scope**
- What are the 3–5 most important features?
- What is explicitly out of scope?
- Any third-party integrations required?

**Technical context**
- Any technology constraints ("must use X" / "cannot use Y")?
- Preferred stack, or should the research agent decide?
- Deployment target: web, mobile, desktop, API, CLI?
- Any performance, security, or compliance requirements?

Once you have enough to write a clear one-paragraph project brief, confirm your understanding before generating anything:

```
Project: <name>
Goal: <one sentence>
Users: <who>
Core features: <bullet list>
Stack: <specified or "to be determined by research agent">
Constraints: <any, or "none stated">
```

Ask the user to confirm before proceeding.

---

## Phase 2: Pre-flight Checks

Ask the user the following three questions in a single message:

> Before generating the agents, a few quick questions about your setup:
>
> 1. Do you have the **context7 plugin** enabled? (Used by the research agent for live documentation lookup — `/plugin install context7@claude-plugins-official` if not)
> 2. Do you have **Figma MCP** configured? (Used by the mockup agent to create wireframes programmatically)
> 3. Do you want to use **parallel agent teams** for code generation? (Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` — faster but experimental)

Wait for the user's answers. Note each tool's status (enabled / not configured) for use when generating the agents.

Do not modify any settings here. Embed the relevant status and setup instructions directly into each generated agent that needs them.

---

## Phase 3: Generate Agent Files

Create `.claude/agents/` in the project root if it does not exist.

For each agent, read the corresponding template from `${CLAUDE_SKILL_DIR}/templates/`, then generate a customised version with project-specific context written in. Write each file to `.claude/agents/<name>.md`.

Template files and what to customise in each:

### `${CLAUDE_SKILL_DIR}/templates/requirements.md` → `.claude/agents/requirements.md`
Embed: project summary, known features and constraints (so the agent skips what's already known), existing files to review first, domain-specific probing questions.

### `${CLAUDE_SKILL_DIR}/templates/research.md` → `.claude/agents/research.md`
Embed: project type and domain, any stack preferences as hard constraints, deployment target, known integrations to research. If context7 is **not** enabled, add setup instructions at the top of the agent (see template).

### `${CLAUDE_SKILL_DIR}/templates/mockup.md` → `.claude/agents/mockup.md`
Embed: project type (determines screen set), known UI requirements. If Figma MCP is **not** configured, add setup instructions at the top of the agent (see template).

### `${CLAUDE_SKILL_DIR}/templates/codegen.md` → `.claude/agents/codegen.md`
Embed: project type and likely component breakdown, stack preference if known. If agent teams flag is **not** set, embed instructions for the agent to add it before spawning teams.

### `${CLAUDE_SKILL_DIR}/templates/qa.md` → `.claude/agents/qa.md`
Embed: tech stack (determines test frameworks), security focus areas specific to this project type, any compliance requirements.

See `${CLAUDE_SKILL_DIR}/examples/sample-pipeline.md` for an example of the expected output quality.

---

## Phase 4: Write PIPELINE.md

Write `PIPELINE.md` to the project root (not inside `.claude/agents/`):

```markdown
# Dev Pipeline

Generated: <date>

## Project
<one-paragraph project summary>

## Agent Sequence

| Step | Agent | Invoke | Output |
|---|---|---|---|
| 1 | Requirements | `/dev-pipeline:requirements` | `PRD.md` |
| 2 | Research | `/dev-pipeline:research` | `RESEARCH.md` |
| 3 | Mockup | `/dev-pipeline:mockup` | `MOCKUP.md` + Figma board |
| 4 | Code Generation | `/dev-pipeline:codegen` | Source code |
| 5 | QA & Testing | `/dev-pipeline:qa` | `QA.md`, `TESTS.md` |

## Prerequisites

| Tool | Status | Notes |
|---|---|---|
| context7 MCP | <enabled / not enabled> | Used by research agent — setup instructions inside agent if needed |
| Figma MCP | <configured / not configured> | Used by mockup agent — setup instructions inside agent if needed |
| Agent Teams | <set / not set> | Used by codegen agent — agent sets it automatically if missing |

## Files
All agents are in `.claude/agents/` and are pre-loaded with context for this project.
```

---

## Phase 5: Report to User

Tell the user:

> **Pipeline agents generated.**
>
> Five agents are ready in `.claude/agents/`, each pre-loaded with context for this project.
>
> Run them in order using the pipeline runner skills:
>
> | Step | Skill | Output |
> |---|---|---|
> | 1 | `/dev-pipeline:requirements` | `PRD.md` |
> | 2 | `/dev-pipeline:research` | `RESEARCH.md` |
> | 3 | `/dev-pipeline:mockup` | `MOCKUP.md` + Figma board |
> | 4 | `/dev-pipeline:codegen` | Source code |
> | 5 | `/dev-pipeline:qa` | `QA.md`, `TESTS.md` |
>
> Each runner checks prerequisites automatically and tells you what to run next.
>
> **Start now → `/dev-pipeline:requirements`**

Flag any missing prerequisites (context7, Figma MCP, agent teams) clearly so the user knows what to set up before reaching that step.
