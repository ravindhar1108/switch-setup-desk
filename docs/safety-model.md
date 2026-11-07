# Safety Model

Allowing AI agents to run terminal commands can be dangerous if there are no boundaries. Setup Desk follows simple, practical rules so it doesn't break your machine.

---

## The Golden Rule: Stay Inside the Workspace

All agent activity is restricted to a dedicated project folder (e.g. `./workspace/project-name/`).

```
Your Machine
├── C:\ / /home/ (Protected: No agent access)
├── Credentials & SSH keys (Protected: No agent access)
│
└── ./workspace/project/  <── ONLY HERE
    ├── .venv/ (Virtual environment)
    ├── cloned repo
    └── SETUP.md
```

---

## What Agents Are Allowed to Do

- Read repo documentation and manifests.
- Clone public repositories into the workspace folder.
- Create local virtual environments (`python -m venv .venv`).
- Install dependencies inside the virtual environment (`pip install`, `npm install`).
- Run tests (`pytest`, `npm test`) and launch the app in dry-run or `--help` mode.
- Make safe local edits to fix errors (e.g. relaxing a version pin or creating `.env`).
- Write `SETUP.md`.

---

## What Agents Are NEVER Allowed to Do

If an agent needs any of these, it **must stop immediately** and notify you in Slack:

- **No sudo or admin rights:** Never runs `sudo`, `su`, or administrator commands.
- **No global installs:** Never runs `npm install -g`, `apt install`, or edits system Python.
- **No leaving the workspace:** Never reads or writes files outside the project directory.
- **No credential access:** Never reads SSH keys (`~/.ssh`), AWS credentials, or system password vaults.
- **No destructive commands:** Never runs `rm -rf /` or kills host processes.

---

## A Note on Real Security

Agent prompts alone do not guarantee security. Hard boundaries must be enforced by the runtime environment:
- Switch sandboxing.
- Codex / OpenCode tool execution permissions.
- Running inside a non-root user account or container.
