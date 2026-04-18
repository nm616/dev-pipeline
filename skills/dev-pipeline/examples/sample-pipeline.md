# Example: Generated Agent Files for "Invoicer"

This example shows what the dev-pipeline skill produces for a freelance invoicing web app.

---

## Sample PIPELINE.md

```markdown
# Dev Pipeline

Generated: 2026-04-18

## Project
Invoicer is a web app for freelancers to manage clients, track projects, generate PDF
invoices, and monitor payment status. Built for solo operators and small agencies who
currently manage invoices in spreadsheets or Word docs.

## Agent Sequence

| Step | Agent | Invoke | Output |
|---|---|---|---|
| 1 | Requirements | `/requirements` | `PRD.md` |
| 2 | Research | `/research` | `RESEARCH.md` |
| 3 | Mockup | `/mockup` | `MOCKUP.md` + Figma board |
| 4 | Code Generation | `/codegen` | Source code |
| 5 | QA & Testing | `/qa` | `QA.md`, `TESTS.md` |

## Prerequisites

| Tool | Status | Notes |
|---|---|---|
| context7 MCP | enabled | Ready |
| Figma MCP | not configured | Setup instructions inside mockup agent |
| Agent Teams | not set | Codegen agent sets it automatically |

## Files
All agents are in `.claude/agents/` and pre-loaded with context for Invoicer.
```

---

## Sample generated agent: `.claude/agents/requirements.md`

```markdown
---
name: requirements
description: Requirements agent for Invoicer. Interviews the user and produces PRD.md.
effort: high
---

# Requirements Agent — Invoicer

You are a senior product manager working on **Invoicer**, a web app for freelancers
to manage clients, track projects, generate PDF invoices, and monitor payment status.

## What We Already Know

- Target users: freelancers and small agencies (solo to ~5 people)
- Core problem: invoice management is currently done in spreadsheets or Word — tedious and error-prone
- Deployment: web app
- Stack: no preference stated — research agent will decide

## Existing Files to Review First
None — this is a greenfield project.

## Interview

Skip questions whose answers are already clear above. Focus on:

**Features**
- Should clients have a portal to view/pay invoices, or is this internal-only?
- What invoice line items are needed — time, fixed fee, expenses, or all three?
- Does PDF generation need custom branding (logo, colour)?
- What payment tracking means: manual status updates, or integration with Stripe/PayPal?

**Users**
- Does the app need multiple user accounts, or is it single-user?
- Any team collaboration features (shared clients, invoice approval)?

**Data & compliance**
- What countries will users be in? (Affects tax/VAT fields on invoices)
- Any data retention requirements?

**Constraints**
- Any budget constraints that affect hosting choice?
- Must the app work offline?

...
```

---

## What Makes a Good Generated Agent

The key difference between a generic template and a well-generated agent:

| Generic | Good (project-specific) |
|---|---|
| "Ask about the target users" | "Ask whether clients need a payment portal, since the brief mentions payment tracking" |
| "Check for compliance requirements" | "Ask about EU/US tax rules since invoicing has VAT/GST implications" |
| "Research frontend frameworks" | "Research React vs Next.js — SSR matters if invoices need to be publicly shareable via URL" |
| "Create screens for key flows" | "Create: dashboard, client list, invoice editor, invoice preview, payment status view" |

The skill reads the project context and embeds this specificity into each agent — so each agent starts knowing what to focus on rather than asking from scratch.
