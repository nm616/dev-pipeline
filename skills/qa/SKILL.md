---
name: qa
description: Run the QA and testing agent for this project. Step 5 of the dev-pipeline workflow. Reviews the codebase for security issues and quality, creates and runs a full test suite, producing QA.md and TESTS.md. Use after code generation is complete.
disable-model-invocation: true
effort: high
allowed-tools: Read Write Bash
---

# Dev Pipeline — Step 5: QA & Testing

```!
echo "=== Dev Pipeline: Step 5 of 5 — QA & Testing ==="
if [ -f ".claude/agents/qa.md" ]; then
  echo "✓ QA agent found"
else
  echo "✗ QA agent not found — run /dev-pipeline first"
fi
if [ -f "PRD.md" ]; then
  echo "✓ PRD.md found"
else
  echo "✗ PRD.md missing — run /dev-pipeline:requirements first"
fi
# Check for source code (any common project indicators)
if [ -f "package.json" ] || [ -f "pyproject.toml" ] || [ -f "Cargo.toml" ] || [ -f "go.mod" ] || [ -d "src" ] || [ -d "app" ] || [ -d "lib" ]; then
  echo "✓ Source code found"
else
  echo "! No obvious source code detected — run /dev-pipeline:codegen first"
fi
if [ -f "QA.md" ]; then
  echo "! QA.md already exists — running this will overwrite it"
else
  echo "✓ No existing QA.md"
fi
```

If the QA agent is **not found**: stop and tell the user to run `/dev-pipeline` first to generate agents.

If no source code is detected: confirm with the user before proceeding — the code generation step may not have run yet.

If prerequisites are **present**: read `.claude/agents/qa.md` in full and execute those instructions now. That file contains all the details — follow them completely, including fixing issues found directly in source files and running the full test suite.

---

When the QA agent finishes and both `QA.md` and `TESTS.md` have been written, tell the user:

> **Pipeline complete!** 🎉
>
> All five stages have finished. Here's what was produced:
>
> | File | Contents |
> |---|---|
> | `PRD.md` | Product requirements |
> | `RESEARCH.md` | Tech stack decisions |
> | `MOCKUP.md` | Design decisions + Figma URL |
> | `INTERFACES.md` | Internal API contracts |
> | `QA.md` | Security review + code quality findings |
> | `TESTS.md` | Test suite results |
>
> Review `QA.md` for any unresolved issues that need your attention before shipping.
