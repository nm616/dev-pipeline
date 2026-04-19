---
description: Run the mockup agent for this project. Step 3 of the dev-pipeline workflow. Creates a visual wireframe in Figma using MCP and loops until approved, producing MOCKUP.md. Use when research is done and the user wants to design the UI.
disable-model-invocation: true
---

# Dev Pipeline — Step 3: Mockup

```!
echo "=== Dev Pipeline: Step 3 of 5 — Mockup ==="
if [ -f ".claude/agents/mockup.md" ]; then
  echo "✓ Mockup agent found"
else
  echo "✗ Mockup agent not found — run /dev-pipeline:dev-pipeline first"
fi
if [ -f "PRD.md" ]; then
  echo "✓ PRD.md found"
else
  echo "✗ PRD.md missing — run /dev-pipeline:requirements first"
fi
if [ -f "RESEARCH.md" ]; then
  echo "✓ RESEARCH.md found"
else
  echo "✗ RESEARCH.md missing — run /dev-pipeline:research first"
fi
if [ -f "MOCKUP.md" ]; then
  echo "! MOCKUP.md already exists — running this will overwrite it"
else
  echo "✓ No existing MOCKUP.md"
fi
```

If anything is **missing** above: stop and tell the user exactly what to run first based on what's missing.

If all prerequisites are **present**: read `.claude/agents/mockup.md` in full and execute those instructions now. That file contains all the details — follow them completely, including the Figma MCP setup check and the approval loop with the user.

---

When the mockup agent finishes, the user has approved the design, and `MOCKUP.md` has been written, tell the user:

> **Step 3 complete.** `MOCKUP.md` has been created and the mockup is approved.
>
> **Next → `/dev-pipeline:codegen`** — generates the full project codebase based on your PRD, research, and approved mockup.
