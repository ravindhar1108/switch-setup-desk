# Setup Desk Workflows

Setup Desk defines two primary execution paths orchestrated via [Switch](https://github.com/sandbox-quantum/switch) and triggered through Slack:

1. [**Standard Setup Workflow (`setup-workflow.md`)**](setup-workflow.md)
   The baseline end-to-end path executed when a target repository sets up cleanly without fatal errors. Scout gathers intelligence, Setup provisions the isolated workspace, installs dependencies, verifies runtime, and produces `SETUP.md`.

2. [**Self-Healing Recovery Workflow (`self-healing-workflow.md`)**](self-healing-workflow.md)
   The resilient recovery path triggered whenever Setup encounters an installation, compilation, or runtime error. Setup transmits the failure context to Repair, which diagnoses the root cause, applies safe workspace-level corrections, and hands back to Setup for verification (bounded by a maximum of 3 repair attempts).

---

## Workflow Comparison Matrix

| Dimension | Standard Setup Workflow | Self-Healing Recovery Workflow |
|---|---|---|
| **Trigger** | Clean user setup request via Slack | Build, dependency, or runtime error during Setup execution |
| **Active Agents** | Scout $\rightarrow$ Setup | Scout $\rightarrow$ Setup $\rightleftharpoons$ Repair (Loop) |
| **Max Retries** | 1 (direct path) | Up to 3 bounded repair attempts per failure |
| **Repair Participation** | Inactive | Active diagnosis and targeted file remediation |
| **Final Artifacts** | `SETUP.md` + Slack notification | `SETUP.md` with repair audit log OR Failure diagnosis report |

---

## Detailed Specifications

- Read [`workflows/setup-workflow.md`](setup-workflow.md) for the happy-path execution flow.
- Read [`workflows/self-healing-workflow.md`](self-healing-workflow.md) for the failure detection, remediation, retry loop, and safety boundaries.

