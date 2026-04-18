---
name: codegen
description: Run the code generation agent for this project. Step 4 of the dev-pipeline workflow. Generates the full codebase using agent teams where efficient. Use when the mockup is approved and the user is ready to generate code.
disable-model-invocation: true
effort: max
allowed-tools: Read Write Bash
---

# Dev Pipeline — Step 4: Code Generation

```!
echo "=== Dev Pipeline: Step 4 of 5 — Code Generation ==="
if [ -f ".claude/agents/codegen.md" ]; then
  echo "✓ Codegen agent found"
else
  echo "✗ Codegen agent not found — run /dev-pipeline first"
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
  echo "✓ MOCKUP.md found"
else
  echo "✗ MOCKUP.md missing — run /dev-pipeline:mockup first"
fi
```

If anything is **missing** above: stop and tell the user exactly what to run first based on what's missing.

If all prerequisites are **present**: read `.claude/agents/codegen.md` in full and execute those instructions now. That file contains all the details — follow them completely, including the agent teams pre-flight check, INTERFACES.md creation, and the team vs. sequential decision.

This is the longest step. It may spawn multiple subagents working in parallel. Keep the user informed of progress.

---

When code generation is complete, tell the user:

> **Step 4 complete.** The codebase has been generated.
>
> **Next → `/dev-pipeline:qa`** — reviews the code for security issues, quality, and coverage, then runs a full test suite.
