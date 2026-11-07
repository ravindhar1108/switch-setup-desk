# Setup Desk Agents

Instead of giving one AI agent a giant prompt and hoping it doesn't get confused, Setup Desk splits the work between three focused agents in [Switch](https://github.com/sandbox-quantum/switch):

| Agent | Job | Permissions |
|---|---|---|
| [**Scout**](scout.md) | Reads the repo, documentation, and GitHub PRs/issues to catch gotchas before doing anything. | **Read-only**. Never runs commands or edits files. |
| [**Setup**](setup.md) | Clones the repo, creates a virtual environment, installs packages, runs tests, and verifies the app works. | **Workspace only**. Installs and runs things inside the project folder. |
| [**Repair**](repair.md) | Steps in when setup fails. Inspects the error, applies a safe fix, and hands back to Setup (max 3 tries). | **Workspace only**. Fixes config/versions safely. |

---

## Why split them up?

- **Keep context clean:** Scout can read a massive README and 10 GitHub issues without filling up Setup's memory with useless text.
- **Safer by default:** Scout doesn't have write permissions, so it physically can't run sketchy scripts while browsing repos.
- **Better debugging:** When a build breaks, Setup stops. It doesn't guess or make random edits. It gives the exact error trace to Repair, which specializes in diagnosing errors.

---

## Agent Details

- [`scout.md`](scout.md) — Pre-setup intelligence gathering.
- [`setup.md`](setup.md) — Running installs and verifying builds.
- [`repair.md`](repair.md) — Failure diagnosis and self-healing.
