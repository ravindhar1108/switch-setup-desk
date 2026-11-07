# Getting Started

Setup Desk runs inside [Switch](https://github.com/sandbox-quantum/switch) and connects to Slack. It isn't a standalone `npm` or `pip` package; it's a multi-agent workflow configured in the Switch Console.

---

## What You Need

1. **Switch:** An active [Switch](https://github.com/sandbox-quantum/switch) environment.
2. **Agent Backends:** Codex and OpenCode connected to your Switch workspace.
3. **Slack:** A Slack channel with the Switch integration active.
4. **Local Workspace:** A folder on your machine where projects can be cloned and virtual environments created.

---

## Setup Checklist (Switch Console)

- [ ] **1. Connect Backends:** Ensure Codex and OpenCode backends are active in Switch.
- [ ] **2. Add the Agents:**
  - **Scout:** Add with prompt from [`agents/scout.md`](../agents/scout.md) (Set to read-only).
  - **Setup:** Add with prompt from [`agents/setup.md`](../agents/setup.md) (Allow workspace write/run).
  - **Repair:** Add with prompt from [`agents/repair.md`](../agents/repair.md) (Allow workspace write).
- [ ] **3. Set Up Room & Handoffs:**
  - Slack message $\rightarrow$ runs Scout.
  - Scout report $\rightarrow$ runs Setup.
  - Setup error $\rightarrow$ triggers Repair (max 3 retries).
  - Repair fix $\rightarrow$ hands back to Setup.
- [ ] **4. Link Slack Channel:** Connect the Switch room to a Slack channel (e.g. `#setup-desk`).
- [ ] **5. Set Workspace Path:** Point Setup to an isolated folder (e.g. `./workspace/`).

---

## Running a Setup in Slack

Once configured, post in your Slack channel:

```text
@SetupDesk Set up https://github.com/pallets/flask in my workspace. Investigate known setup issues first, install dependencies, run the project, and recover from safe setup failures if necessary.
```

### What happens in the thread:
1. Scout posts a summary of the repo, required Python version, and known issues.
2. Setup confirms it created a `.venv` and started installing packages.
3. If an install fails, Repair posts what broke, applies a local fix, and lets Setup retry.
4. When done, Setup posts a message confirming the app runs and shares the generated `SETUP.md`.
