# Self-Healing Recovery Workflow

The self-healing loop is Setup Desk's main feature. When an install or build fails, Setup Desk doesn't just crash. It diagnoses what broke, fixes it locally, and retries.

---

## Flowchart

```mermaid
flowchart TD
    User["User in Slack"] -->|"1. Setup Request"| Slack["Slack"]
    Slack -->|"2. Start"| Switch["Switch"]
    
    Switch -->|"3. Intel"| Scout["Scout Agent"]
    Scout -->|"4. Notes"| Setup["Setup Agent"]
    
    Setup -->|"5. Run install / test"| Step{"Did it work?"}
    
    %% Success Path
    Step -- Yes --> Verify["Verify app runs & write SETUP.md"]
    Verify --> Done["Post success in Slack"]
    
    %% Self-Healing Loop
    Step -- No --> CheckRetries{"Retries < 3?"}
    
    CheckRetries -- "Yes (Try 1-3)" --> Repair["Repair Agent"]
    Repair --> Diagnose["Diagnose root cause"]
    Diagnose --> Patch["Apply safe workspace fix"]
    Patch -->|"Retry"| Setup
    
    %% Failure Stop
    CheckRetries -- "No (3 attempts failed)" --> Stop["Stop & report exact error to Slack"]
```

---

## Communication Sequence

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Developer
    participant Slack as Slack Channel
    participant Switch as Switch Platform
    participant Scout as Scout Agent
    participant Setup as Setup Agent
    participant Repair as Repair Agent

    Dev->>Slack: Drop repo link & request setup
    Slack->>Switch: Forward request
    Switch->>Scout: Investigate repo
    Scout-->>Switch: Return Scout report (docs, PRs, issues)
    
    Switch->>Setup: Start setup with Scout notes
    Setup->>Setup: Clone repo & create local venv
    Setup->>Setup: Run pip/npm install (FAILS)
    
    critical Self-Healing Loop (Max 3 Tries)
        Setup->>Switch: Report failure (exit code & stderr)
        Switch->>Repair: Diagnose & fix error
        Repair->>Repair: Check trace & apply local fix
        Repair-->>Switch: Hand fix details back to Setup
        Switch->>Setup: Retry failed command
    end

    Setup->>Setup: Verify app runs & write SETUP.md
    Setup-->>Switch: Done report
    Switch-->>Slack: "Setup verified! Here is your SETUP.md"
    Slack-->>Dev: Ready to code!
```

---

## How It Recovers

1. **Failure happens:** An install fails (e.g. pinned dependency incompatible with your Python/Node version, or missing `.env`).
2. **Error passed to Repair:** Setup grabs the failed command and stderr log and hands them to Repair in Switch.
3. **Repair checks root cause:** Repair looks at the error and checks if Scout flagged an issue for this.
4. **Safe local patch:** Repair fixes the local file (like updating a version constraint or copying `.env.example` to `.env`).
5. **Setup retries:** Setup re-runs the command. If it passes, it moves on to testing and verification.
6. **Hard cap of 3 tries:** If the issue still isn't fixed after 3 repair attempts, the workflow stops and tells the developer in Slack what went wrong and what needs human attention.
