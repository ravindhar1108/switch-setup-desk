# Technology Stack

Setup Desk is intentionally designed as an orchestration workflow leveraging proven, open-source and developer-native platforms. It does not introduce a custom frontend web application, proprietary microservices, or custom database backends.

---

## Core Components & Technologies

| Layer | Technology | Primary Role in Setup Desk |
|---|---|---|
| **Multi-Agent Orchestration** | [**Switch**](https://github.com/sandbox-quantum/switch) | Central control engine that provisions multi-agent rooms, coordinates state transitions, enforces retry limits, and manages agent handoffs. |
| **User Interaction Interface** | **Slack** | Conversational UI where developers post setup requests and receive progress alerts, error summaries, and verified `SETUP.md` documentation. |
| **Connected Agent Backends** | **Codex & OpenCode** | Large language model coding-agent backends that execute analytical reasoning, codebase exploration, command formulation, and file patching. |
| **Repository Intelligence** | **GitHub** | Primary source for repository source trees, documentation (`README.md`, `CONTRIBUTING.md`), and public issue/PR discussions containing setup solutions. |
| **Target Execution Environment** | **Local Project Workspace** | Sandboxed filesystem directory where code is cloned, virtual environments are created, and builds/tests are verified. |

---

## Architectural Role Breakdown

### 1. Multi-Agent Orchestration: Switch
[Switch](https://github.com/sandbox-quantum/switch) is the foundational orchestrator for the entire workflow. Switch manages:
- **Agent Lifecycle**: Activating the [Scout](../agents/scout.md), [Setup](../agents/setup.md), and [Repair](../agents/repair.md) agents at the correct workflow steps.
- **Shared Memory & State**: Providing a unified context room where diagnostic logs and investigation reports persist across agent transitions.
- **Handoff Coordination**: Routing messages between Slack, Scout, Setup, and Repair based on execution events (e.g., dispatching error traces to Repair on failure).
- **Retry Enforcement**: Bounding the self-healing cycle to a maximum of 3 repair attempts per failure sequence.

### 2. User Interaction: Slack
Slack serves as the conversational interface for Setup Desk:
- Eliminates the need for a separate custom web dashboard or browser client.
- Developers initiate setup tasks using natural language requests directly in their existing team channels.
- Formatted summaries, error breakdowns, and completed setup artifacts are returned within the same conversational thread.

### 3. Agent Backends: Codex and OpenCode
Setup Desk delegates code reasoning and execution to connected agent backends:
- **Codex & OpenCode** interpret project files, generate shell commands for the target operating system, analyze execution traces, and produce targeted file fixes during repair cycles.
- Operating under assigned toolsets and system instructions defined in the `agents/` specifications.

### 4. Repository Intelligence: GitHub
Rather than relying purely on pre-trained model knowledge, Setup Desk looks at live repository metadata:
- README files, contribution guides, and package configuration files.
- Public issue discussions and pull request descriptions to identify known bugs and community workarounds before or during setup.

### 5. Target Execution Environment: Local Workspace
The local host or containerized workspace directory provides the actual runtime substrate:
- Houses cloned repositories.
- Isolates dependencies inside project-local virtual environments (e.g., Python `venv`).
- Runs compilation, test suites, and runtime verification.

---

## Explicit Architecture Clarification

To maintain transparency and accuracy:
- **No Custom Web Frontend**: Setup Desk does not use React, Vue, Angular, or any custom web GUI. Interaction takes place in Slack.
- **No Custom Database**: Setup Desk does not utilize PostgreSQL, MongoDB, SQLite, or Redis. Session context is managed natively through the Switch room.
- **No Custom Web Backend**: Setup Desk does not run a custom FastAPI, Express, or Django server; orchestration logic is powered entirely through Switch.

