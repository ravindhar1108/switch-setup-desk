# Setup Desk: 2–3 Minute Hackathon Demo Script

**Hackathon**: Flip the Switch Hackathon  
**Track**: Business Workflow  
**Project**: Setup Desk — Your AI-powered assistant for setting up open-source projects.

---

## Video / Presentation Breakdown

| Timestamp | Section | Visual Focus |
|---|---|---|
| **0:00 – 0:35** | Introduction, Problem & Personal Story | Presenter / Title Slide |
| **0:35 – 0:55** | The Solution & Architecture | Architecture Diagram & Switch Console |
| **0:55 – 1:20** | Scout: Pre-Setup Intelligence | Slack Request & Scout Output |
| **1:20 – 1:45** | Setup & The Self-Healing Failure | Setup Running in Workspace & Error Log |
| **1:45 – 2:20** | Repair & Successful Verification | Repair Diagnosis, Fix & Setup Retry |
| **2:20 – 2:45** | Outcome, Verification & Wrap-up | Final `SETUP.md` & Slack Delivery |

---

## Spoken Script

### [0:00 – 0:35] Introduction, Problem & Personal Story
*(Speaker on camera or showing title slide)*

"Hi everyone! I’m excited to present **Setup Desk**, our project for the **Flip the Switch Hackathon** in the Business Workflow track.

Every developer has been there: you find an exciting open-source project on GitHub, clone it, and spend the next three hours wrestling with broken installs, missing dependencies, and outdated README instructions.

The motivation for this project actually came directly from my own experience. While trying to run Switch on Windows, I ran into an issue where the Antigravity integration wouldn’t work. I spent hours troubleshooting, only to find out from the maintainer that the fix was already sitting right there in an open pull request!

That made me realize: developers waste countless hours debugging setup problems that community members have already solved. That's why I built Setup Desk."

---

### [0:35 – 0:55] The Solution & Architecture
*(Show Architecture Diagram or Switch Console)*

"Setup Desk is an AI-powered assistant for setting up open-source repositories locally. 

Instead of manual troubleshooting, you simply submit a repository link in Slack. Behind the scenes, **Switch** orchestrates three specialized agents powered by Codex and OpenCode:

1. **Scout** — which investigates before we touch anything.
2. **Setup** — which handles isolated installation and runtime verification.
3. **Repair** — our self-healing agent that diagnoses and fixes errors."

---

### [0:55 – 1:20] Scout: Pre-Setup Intelligence
*(Show Slack message and Scout's output)*

"Let’s see it in action. In our `#setup-desk` Slack channel, I ask:  
*‘Set up pallets/flask in my workspace. Investigate known setup issues first, install dependencies, run the project, and recover from safe setup failures if necessary.’*

Immediately, Switch initiates the room and triggers **Scout**.

Notice that Scout is strictly read-only. It scans the repository files, checks prerequisites, and searches public GitHub issues and pull requests to flag known compatibility traps on our host machine. It bundles this into a clean intelligence report for Setup."

---

### [1:20 – 1:45] Setup & The Failure
*(Show Setup terminal execution and error output)*

"Next, Switch hands control over to **Setup**. 

Setup creates an isolated workspace, provisions a project-local virtual environment, and begins installing dependencies. Notice that Setup never touches global system packages.

Now watch what happens when an installation error occurs—here, an outdated dependency version fails on Python 3.11. In a traditional script, everything would just crash. But Setup detects the error, halts execution, and immediately hands the complete diagnostic logs over to **Repair**."

---

### [1:45 – 2:20] Repair & The Self-Healing Loop
*(Show Repair diagnosis and Setup retry in Switch room)*

"This is Setup Desk’s core differentiator: the **Self-Healing Loop**.

Repair steps in, parses the traceback, and identifies that relaxing a patch-level pin in the workspace configuration fixes the issue safely. 

Crucially, Repair enforces strict safety boundaries: no admin rights, no system-level modifications, and a hard cap of three repair attempts to avoid runaway loops.

Repair applies the targeted fix to the local file, explains exactly what changed and why, and hands control back to Setup. Setup automatically retries the command—and this time, it succeeds!"

---

### [2:20 – 2:45] Outcome & Wrap-up
*(Show verified runtime, `SETUP.md`, and Slack notification)*

"Setup doesn’t stop at package installation—it actually verifies that the test suite passes and that the CLI runs.

Once verified, Setup generates a clean, permanent `SETUP.md` document in the repository root and sends a completion notification right back to our Slack thread.

With Switch orchestrating Scout, Setup, and Repair, setting up an open-source project is no longer an afternoon of frustration—it's an autonomous, self-healing workflow.

Thank you!"

