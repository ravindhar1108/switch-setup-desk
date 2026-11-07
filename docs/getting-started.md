# Getting Started with Setup Desk

This guide explains how Setup Desk is configured and operated. 

> [!NOTE]
> **Implementation Architecture Notice**
> Setup Desk is configured and executed through the **Switch Console** and **Slack**, utilizing connected agent backends (**Codex** and **OpenCode**). 
> 
> This repository contains the architecture specifications, agent role definitions, workflow rules, and reference documentation. It is not a standalone executable application with an npm or pip package install command.

---

## Prerequisites

Before configuring Setup Desk, ensure you have:

1. **Switch Platform Access**: An active account and deployment of [Switch](https://github.com/sandbox-quantum/switch).
2. **Connected Agent Backends**: Authorized access to **Codex** and **OpenCode** agent runtimes connected to your Switch workspace.
3. **Slack Workspace Integration**: A Slack workspace with administrative permissions to install the Switch Slack bot or webhook integration.
4. **Target Execution Environment**: A designated local or containerized workspace directory with network access to clone public repositories and download language packages.

---

## Configuration Checklist (Switch Console)

Follow this checklist within the Switch Console to establish the Setup Desk workflow:

- [ ] **Step 1: Connect Agent Backends**
  - Verify that Codex and OpenCode backends are registered and operational within your Switch settings.
- [ ] **Step 2: Define Agent Profiles**
  - Create the **Scout** agent profile using specifications from [`agents/scout.md`](../agents/scout.md) (Set to read-only mode).
  - Create the **Setup** agent profile using specifications from [`agents/setup.md`](../agents/setup.md) (Grant workspace write/execute permissions).
  - Create the **Repair** agent profile using specifications from [`agents/repair.md`](../agents/repair.md) (Grant workspace write permissions).
- [ ] **Step 3: Configure Shared Room & Context Pipeline**
  - Create a designated Switch Room for setup jobs.
  - Set up handoff triggers:
    - User message in Slack routes to **Scout**.
    - Scout output routes to **Setup**.
    - Setup failure event triggers **Repair**.
    - Repair output routes back to **Setup** (enforcing maximum of 3 retries).
- [ ] **Step 4: Connect Slack Channel**
  - Link the Switch Room to a dedicated Slack channel (e.g., `#setup-desk` or `#dev-workflows`).
  - Grant the integration permissions to read incoming messages and post threaded replies.
- [ ] **Step 5: Define Workspace Boundaries**
  - Bind the agent runner to an isolated filesystem directory (e.g., `/tmp/setup-workspace/` or `./workspace/`).
  - Ensure the host execution runner enforces the restrictions outlined in [`docs/safety-model.md`](safety-model.md).

---

## How to Submit a Setup Request via Slack

Once configured, users trigger Setup Desk directly by posting in the connected Slack channel.

### Recommended Request Format

Submit your request with the repository URL and any specific setup instructions:

```text
Set up https://github.com/pallets/flask in my workspace. Investigate known setup issues first, install dependencies, run the project, and recover from safe setup failures if necessary.
```

### What to Expect Next

1. **Scout Phase**: Setup Desk replies in the Slack thread confirming that Scout has started repository intelligence gathering. Scout scans the repository structure, documentation, issues, and PRs.
2. **Setup Phase**: Setup reports that it has cloned the code into an isolated environment and begun package installation.
3. **Self-Healing Phase (if needed)**: If a dependency or build error occurs, you will see a notification indicating that Repair has intervened to analyze the logs, apply a safe workspace-level fix, and retry.
4. **Completion Delivery**: Upon successful verification, the thread receives:
   - Confirmation that the application builds and runs.
   - A copy of the newly generated `SETUP.md`.
   - Summary of any repairs applied during the run.

---

## Example Walkthroughs

For complete illustrative transcripts and reports:
- [Sample Slack Request](../examples/sample-request.md)
- [Illustrative Scout Report](../examples/scout-report.md)
- [Illustrative Setup Report](../examples/setup-report.md)
- [Illustrative Repair Report](../examples/repair-report.md)

