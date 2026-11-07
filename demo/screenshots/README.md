# Setup Desk Demonstration Screenshots

This directory serves as the designated location for actual visual captures from your Switch Console, Slack workspace, and local project environment.

> [!IMPORTANT]
> **No Synthetic or Fake Screenshots**  
> In adherence to open-source submission integrity, this repository does not include fabricated or simulated mock-up images. Maintainers and contributors should capture real screenshots from live Switch and Slack sessions following the checklist below.

---

## Required Screenshots Checklist

When capturing screenshots for your hackathon submission or GitHub presentation, name and place them in this folder as follows:

| Filename | Capture Location | Description / Content to Display |
|---|---|---|
| `01-slack-setup-request.png` | Slack | The initial user message posted in the Slack channel triggering the repository setup. |
| `02-scout-investigation.png` | Switch Console / Slack | The Scout agent active in the Switch room or posting its initial investigation findings. |
| `03-setup-execution.png` | Terminal / Switch Console | The Setup agent provisioning the local virtual environment and initiating package install. |
| `04-setup-failure.png` | Terminal / Switch Console | The non-zero exit code or dependency collision captured by Setup, triggering the handoff. |
| `05-repair-diagnosis.png` | Switch Console / Slack | The Repair agent analyzing the stack trace, explaining the root cause, and applying a fix. |
| `06-successful-retry.png` | Terminal / Switch Console | The Setup agent re-running the command after repair and running smoke-test verification. |
| `07-generated-setup-md.png` | Code Editor / Slack | The verified `SETUP.md` file produced in the project workspace and delivered to Slack. |

---

## Image Formatting Tips

- **Format**: PNG or WebP.
- **Resolution**: High-DPI / 1080p minimum for readability.
- **Redaction**: Ensure any personal API tokens, private webhook URLs, or sensitive machine paths are masked before committing.

