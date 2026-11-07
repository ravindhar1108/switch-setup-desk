# Sample Request: Slack Setup Trigger

This document illustrates how a developer initiates a setup task using Setup Desk within a connected Slack channel.

> **Note**: This is an illustrative example of the user interaction format.

---

## Slack Interaction Scenario

**Channel**: `#setup-desk`  
**User**: `@alex.dev`  
**Bot Integration**: `@SetupDesk (Switch Bot)`

### User Message

> **@alex.dev**:  
> `@SetupDesk Set up https://github.com/pallets/flask in my workspace. Investigate known setup issues first, install dependencies, run the project, and recover from safe setup failures if necessary.`

---

## What the System Does in Response

1. **Initial Acknowledgment**:  
   `@SetupDesk` immediately replies in a thread:
   > *“Received request for `pallets/flask`. Initializing Switch room session `#sd-flask-702`.*  
   > *Dispatching **Scout** for pre-setup intelligence gathering...”*

2. **Progress Notifications**:  
   As agents transition through handoffs, threaded updates keep the developer informed:
   - Scout finishes investigation $\rightarrow$ posts summary checklist.
   - Setup clones repository $\rightarrow$ provisions project-local virtual environment (`.venv`).
   - Setup encounters a dependency conflict $\rightarrow$ invokes Repair.
   - Repair diagnoses the issue, applies a project-local fix, and passes back to Setup.
   - Setup retries installation, runs test suites, verifies runtime boot, and writes `SETUP.md`.

3. **Final Delivery**:  
   `@SetupDesk` posts the verified `SETUP.md` and a summary of actions taken into the Slack thread.

