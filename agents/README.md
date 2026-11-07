# Setup Desk Agents

Setup Desk divides the task of repository setup and validation across three specialized agents orchestrated by [Switch](https://github.com/sandbox-quantum/switch). Rather than relying on a single monolithic prompt, each agent maintains a dedicated role, strict operational boundaries, and explicit handoff contracts.

---

## Agent Overview

| Agent | Core Role | Primary Responsibility | Permissions & Boundaries |
|---|---|---|---|
| [**Scout**](scout.md) | Pre-Setup Intelligence | Analyzes target repository docs, dependencies, environment constraints, and public GitHub issues/PRs for known pitfalls. | **Read-only**. Never executes build commands or alters code. |
| [**Setup**](setup.md) | Execution & Verification | Clones code, provisions project-local virtual environments, installs dependencies, builds, tests, verifies application runtime, and authors `SETUP.md`. | **Workspace-scoped write & run**. Only acts within designated directory. |
| [**Repair**](repair.md) | Diagnosis & Remediation | Diagnoses build/runtime errors, pinpoints root causes against known fixes, applies safe workspace-level corrections, and hands back to Setup (up to 3 attempts). | **Workspace-scoped repair**. Never uses `sudo` / admin or alters global files. |

---

## The Specialization Advantage

Combining setup automation with multi-agent orchestration yields distinct advantages over single-agent prompts:

1. **Context hygiene**: Pre-setup research (scanning multiple PRs, reading extensive READMEs) remains separated from setup execution logs, preventing token bloat and hallucination.
2. **Defensive separation of duty**: Scout cannot accidentally trigger unverified scripts because it lacks write or execute authority.
3. **Structured failure loops**: When execution fails, Setup does not attempt ad-hoc guesswork; it halts and hands execution context to Repair. Repair diagnoses the exact failure, applies a targeted correction, and hands back control for independent verification.

---

## Agent Specifications

Explore the detailed specifications, inputs, behavioral rules, and handoff contracts for each agent:

- [`agents/scout.md`](scout.md) — Detailed specification for the Scout agent.
- [`agents/setup.md`](setup.md) — Detailed specification for the Setup agent.
- [`agents/repair.md`](repair.md) — Detailed specification for the Repair agent.

