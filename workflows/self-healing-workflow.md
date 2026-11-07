# Workflow: Self-Healing Recovery Loop

The **Self-Healing Recovery Workflow** is the primary differentiator of Setup Desk. Instead of terminating when a command fails or requiring the developer to parse esoteric stack traces, Setup Desk engages a closed-loop diagnosis and remediation cycle coordinated by [Switch](https://github.com/sandbox-quantum/switch).

---

## Complete Self-Healing Flowchart

```mermaid
flowchart TD
    User(["User (Slack)"]) -->|"1. Setup Request"| Slack["Slack Channel"]
    Slack -->|"2. Trigger Workflow"| Switch["Switch Orchestration Platform"]
    
    Switch -->|"3. Pre-Setup Intel"| Scout["Scout Agent"]
    Scout -->|"4. Investigation Report"| Switch
    Switch -->|"5. Begin Setup"| Setup["Setup Agent"]
    
    Setup -->|"6. Run Install / Build / Run"| ExecStep{"Step Succeeded?"}
    
    %% Success Path
    ExecStep -- Yes --> Verify["Verify Application Runtime Execution"]
    Verify --> GenDoc["Generate SETUP.md"]
    GenDoc --> ReportSuccess["Post Success Summary to Slack"]
    ReportSuccess --> Done(["Setup Completed Successfully"])
    
    %% Failure & Self-Healing Loop
    ExecStep -- No --> FailureCheck{"Repair Attempts < 3?"}
    
    FailureCheck -- "Yes (Attempt 1-3)" --> SendRepair["Send Failure Logs & Context to Repair"]
    SendRepair --> Repair["Repair Agent"]
    Repair --> Diagnose["Diagnose Root Cause & Check Safety Boundaries"]
    Diagnose --> SafeFix["Apply Safe Project-Local Workspace Fix"]
    SafeFix --> HandoffBack["Hand Context Back to Setup"]
    HandoffBack --> Setup
    
    %% Retry Limit Exceeded
    FailureCheck -- "No (Exceeded 3 Retries)" --> Unresolved["Mark as Unresolved / Blocked"]
    Unresolved --> ReportFail["Report Detailed Diagnostic & Blocker to Slack"]
    ReportFail --> Failed(["Workflow Halted - Human Assistance Needed"])
```

---

## Communication Sequence Diagram

The interaction sequence between the user, Slack, Switch, and the three agents:

```mermaid
sequenceDiagram
    autonumber
    actor User as Developer
    participant Slack as Slack Channel
    participant Switch as Switch Platform
    participant Scout as Scout Agent
    participant Setup as Setup Agent
    participant Repair as Repair Agent

    User->>Slack: Send setup request (repo URL + parameters)
    Slack->>Switch: Dispatch request payload
    Switch->>Scout: Launch pre-setup investigation
    Scout->>Scout: Inspect docs, manifests, PRs & issues
    Scout-->>Switch: Return Scout Investigation Report
    
    Switch->>Setup: Trigger setup with Scout report & workspace
    Setup->>Setup: Clone repo & create local venv
    Setup->>Setup: Execute dependency install (FAILS)
    
    critical Self-Healing Loop (Max 3 Attempts)
        Setup->>Switch: Report failure (exit code, stderr, command)
        Switch->>Repair: Delegate diagnosis & remediation (Attempt 1 of 3)
        Repair->>Repair: Analyze error stack & inspect project files
        Repair->>Repair: Apply project-local fix (e.g., pin compatible version)
        Repair-->>Switch: Report fix details & request retry
        Switch->>Setup: Hand control back to Setup
        Setup->>Setup: Retry install and execute build / smoke test
    end

    Setup->>Setup: Verify application execution & write SETUP.md
    Setup-->>Switch: Final setup report & documentation
    Switch-->>Slack: Format result & deliver summary
    Slack-->>User: Notification: "Repository setup verified!"
```

---

## Detailed Step-by-Step Breakdown

### 1. Trigger Conditions
The self-healing cycle is triggered immediately upon any non-zero exit code or explicit fatal error during:
- Dependency resolution or installation (e.g., pip wheels failing to compile, npm package conflicts).
- Build compilation or asset bundling.
- Configuration loading (e.g., missing expected environment files).
- Runtime verification (e.g., application crashes on startup).

### 2. Error Information Captured
Setup packages a diagnostic bundle into the Switch room:
- Command line string executed.
- Complete stdout and stderr streams.
- Working directory file path.
- Host environment context (OS, architecture, shell).
- Relevant project configuration files.

### 3. Repair Diagnosis
The [Repair agent](../agents/repair.md) analyzes the failure:
- Distinguishes between transient glitches, configuration oversights, and dependency incompatibilities.
- Checks against known workarounds identified earlier in Scout's investigation report.
- Verifies that any proposed correction falls strictly within the project directory.

### 4. Safe Remediation
Repair applies minimal, project-local modifications:
- Updates a pinned version in `requirements.txt` or `package.json`.
- Creates a local `.env` configuration file from an existing template.
- Adjusts build flags or compatibility settings.

### 5. Handoff & Retry
Repair logs what changed and why, then hands control back to Setup. Setup executes the retry, picking up from the point of failure.

### 6. The 3-Attempt Retry Boundary
Setup Desk strictly enforces a limit of **three repair attempts per failure sequence**.
- If after 3 attempts the failure persists, Setup Desk halts.
- It delivers a comprehensive breakdown to the developer in Slack explaining the root cause, what fixes were attempted, and why human intervention or system-level administrative action is required.

