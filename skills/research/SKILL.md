---
description: Run the research agent for this project. Step 2 of the dev-pipeline workflow. Researches and selects the optimal tech stack, producing RESEARCH.md. Use when requirements are done and the user wants to determine the technology approach.
disable-model-invocation: true
---

# Dev Pipeline — Step 2: Research

```!
echo "=== Dev Pipeline: Step 2 of 5 — Research ==="
if [ -f ".claude/agents/research.md" ]; then
  echo "✓ Research agent found"
else
  echo "✗ Research agent not found — run /dev-pipeline:dev-pipeline first"
fi
if [ -f "PRD.md" ]; then
  echo "✓ PRD.md found"
else
  echo "✗ PRD.md not found — run /dev-pipeline:requirements first"
fi
if [ -f "RESEARCH.md" ]; then
  echo "! RESEARCH.md already exists — running this will overwrite it"
else
  echo "✓ No existing RESEARCH.md"
fi
```

If anything is **missing** above: stop and tell the user exactly what to run first:
- No research agent → run `/dev-pipeline:dev-pipeline` to generate agents
- No `PRD.md` → run `/dev-pipeline:requirements` first

If all prerequisites are **present**: read `.claude/agents/research.md` in full and execute those instructions now. That file contains all the details — follow them completely.

---

When the research agent finishes and `RESEARCH.md` has been written, tell the user:

> **Step 2 complete.** `RESEARCH.md` has been created.
>
> **Next → `/dev-pipeline:mockup`** — creates a visual wireframe in Figma based on your requirements and chosen stack.
