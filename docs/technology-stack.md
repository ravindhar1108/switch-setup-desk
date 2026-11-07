# Tech Stack

Setup Desk uses existing, proven developer tools rather than unnecessary custom frameworks:

| Tool | Role | Why we use it |
|---|---|---|
| [**Switch**](https://github.com/sandbox-quantum/switch) | Multi-agent orchestration | Manages the agent room, context sharing, handoffs, and enforces the 3-retry limit. |
| **Slack** | User interface | Developers already use Slack. Drop a link in a channel, get updates in the thread. |
| **Codex / OpenCode** | Agent backends | Connected models that read code, run shell commands, and diagnose stack traces. |
| **GitHub** | Intelligence source | Live docs, issues, and PR discussions containing solutions to known setup bugs. |
| **Local Workspace** | Execution sandbox | A project folder with a `.venv` so your global machine remains untouched. |

---

## What We DID NOT Build (And Why)

- **No custom web frontend:** Nobody wants another browser tab open. Slack is faster and already where teams work.
- **No custom database:** Switch manages room state and message history natively.
- **No custom server backend:** Switch handles the agent routing and API triggers.
