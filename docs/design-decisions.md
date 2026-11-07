# Design Decisions

This document outlines the architectural and engineering rationale behind the key design decisions in Setup Desk.

---

## 1. Why Use Multiple Specialized Agents Instead of One General-Purpose Agent?

### Context
A common pattern in LLM-based development is to use a single monolithic agent with an expansive prompt responsible for reading docs, installing software, troubleshooting errors, and summarizing output.

### Decision
Setup Desk divides responsibilities among three specialized agents: **Scout**, **Setup**, and **Repair**, orchestrated by [Switch](https://github.com/sandbox-quantum/switch).

### Rationale
- **Context Isolation and Token Hygiene**: Monolithic agents quickly fill their context window with repository docs, package lists, build outputs, and diagnostic traces. Splitting roles ensures that pre-setup research does not pollute the execution context.
- **Role-Based Security**: Different tasks require different permissions. Scout requires strictly read-only access, while Setup and Repair require workspace write and execution permissions. A single agent model would require granting broad permissions across all phases.
- **Predictable Behavior**: Specialized agents with concise, focused instructions follow systematic workflows more reliably than a single agent burdened with multiple conflicting objectives.

---

## 2. Why Investigate Before Installing?

### Context
Many automated setup tools immediately execute commands found in the `README.md` or attempt default package installation commands (such as `npm install` or `pip install .`).

### Decision
Setup Desk requires **Scout** to perform a structured pre-setup investigation of repository structure, documentation, prerequisites, and public GitHub issues/PRs *before* **Setup** executes any shell commands.

### Rationale
- **Preempting Known Failures**: Documentation is frequently outdated. Important setup prerequisites (e.g., specific Python versions, OS-level dependencies, or environment variables) are often buried in pull requests, closed issues, or secondary setup guides.
- **Safety**: Inspecting build scripts and manifests before running them prevents executing unverified or incompatible commands.
- **Informed Execution**: Scout's structured report provides Setup with precise prerequisites, eliminating guesswork during environment provisioning.

---

## 3. Why Use Slack as the Interaction Interface?

### Context
Many developer tools build custom web dashboards, browser extensions, or desktop GUIs to interface with users.

### Decision
Setup Desk utilizes **Slack** as its primary user-facing interface, mediated through Switch's integration capabilities.

### Rationale
- **Native Developer Workflow**: Software teams already spend their working hours in chat tools like Slack. Meeting developers where they already collaborate lowers the barrier to adoption.
- **Asynchronous Execution**: Repository setup, dependency installation, and build verification can take several minutes. Slack's threaded messaging model accommodates asynchronous job submission and notification naturally.
- **Zero Interface Maintenance**: Eliminating a custom frontend framework reduces project complexity, avoids UI maintenance overhead, and focuses engineering effort on multi-agent orchestration.

---

## 4. Why Introduce a Separate Repair Agent?

### Context
When a setup command fails, the agent executing the command could theoretically attempt to fix its own mistake.

### Decision
Setup Desk separates execution ([Setup](../agents/setup.md)) from failure diagnosis and remediation ([Repair](../agents/repair.md)).

### Rationale
- **Mental Shift Between Execution and Diagnosis**: Execution requires adhering strictly to a verified script or plan. Remediation requires forensic analysis: parsing stack traces, identifying missing packages, checking compatibility matrices, and determining minimal interventions.
- **Preventing Compounding Errors**: Agents that attempt to immediately patch their own runtime errors often make hasty assumptions, modify unrelated files, or enter erratic editing loops. A dedicated Repair agent approaches the failure with fresh diagnostic instructions.
- **Independent Verification**: By returning remediated files back to Setup for re-execution rather than letting Repair run the fix itself, Setup provides an independent verification check on Repair's changes.

---

## 5. Why Use a Bounded Retry Loop?

### Context
Automated repair mechanisms can either halt on the first error or run indefinitely until the system passes or runs out of credits.

### Decision
Setup Desk implements a **bounded retry loop with a strict limit of three repair attempts per failure sequence**.

### Rationale
- **Loop Termination**: Infinite loops are dangerous and costly in autonomous multi-agent systems. If an issue cannot be resolved within three iterations, it typically indicates a missing system-level dependency, an incompatible host OS, or an architectural blocker requiring human decision-making.
- **Sufficient for Common Issues**: Three attempts provide adequate leeway to address cascading project-local issues (e.g., creating a missing `.env`, followed by relaxing a pinned package version, followed by supplying a required build flag) without runaway execution.

---

## 6. Why Generate `SETUP.md`?

### Context
Once an agent sets up a repository, the knowledge gained during that session can easily be lost when the ephemeral session ends.

### Decision
The Setup agent synthesizes its verified actions, local configurations, and any repair steps into a clean, permanent `SETUP.md` file in the project workspace.

### Rationale
- **Persistent Documentation**: The developer receives a concrete, human-readable reference for how the project was successfully set up on their machine.
- **Reproducibility**: If other team members need to run the same project, they can follow the verified instructions directly without re-running the full multi-agent workflow.
- **Auditability**: It provides transparency into what environment variables were created, what flags were used, and what adjustments Repair made.

---

## 7. Why Enforce Workspace and Permission Boundaries?

### Context
Developers often run setup scripts with elevated privileges (e.g., `sudo make install` or global `npm install -g`) to quickly resolve permission errors.

### Decision
Setup Desk enforces strict workspace isolation: agents may only modify files and provision virtual environments within the assigned project directory, and are strictly prohibited from using elevated privileges or modifying global system settings.

### Rationale
- **Host Integrity**: Autonomous agents should never risk destabilizing the host system, overwriting system binaries, or altering global environment configurations.
- **Security Defense-in-Depth**: Confining agents to project-local operations prevents malicious or malformed scripts in unfamiliar open-source repositories from compromising the developer's machine or accessing private credentials.

