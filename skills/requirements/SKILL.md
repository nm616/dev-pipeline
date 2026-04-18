---
name: requirements
description: Run the requirements agent for this project. Step 1 of the dev-pipeline workflow. Conducts a thorough requirements interview and produces PRD.md. Use when the user wants to start the pipeline, gather requirements, or create a PRD for their project.
disable-model-invocation: true
effort: high
allowed-tools: Read Write Bash
---

# Dev Pipeline — Step 1: Requirements

```!
echo "=== Dev Pipeline: Step 1 of 5 — Requirements ==="
if [ -f ".claude/agents/requirements.md" ]; then
  echo "✓ Requirements agent found"
else
  echo "✗ Requirements agent not found"
  echo "  Run /dev-pipeline first to generate agents for this project."
fi
if [ -f "PRD.md" ]; then
  echo "! PRD.md already exists — running this will overwrite it"
else
  echo "✓ No existing PRD.md"
fi
```

If the requirements agent was **not found** above: stop, tell the user to run `/dev-pipeline` first to generate the project agents, then come back.

If the requirements agent **was found**: read `.claude/agents/requirements.md` in full and execute those instructions now. That file contains all the details — follow them completely.

---

When the requirements agent finishes and `PRD.md` has been written, tell the user:

> **Step 1 complete.** `PRD.md` has been created.
>
> **Next → `/dev-pipeline:research`** — researches the best tech stack based on your requirements.
