# 2–3 Minute Demo Video Script

**Hackathon:** Flip the Switch Hackathon  
**Track:** Business Workflow  
**Project:** Setup Desk  

---

## Video Outline

| Time | What you're saying | What you're showing |
|---|---|---|
| **0:00 – 0:30** | The problem & personal story | You on camera or title slide |
| **0:30 – 0:50** | The solution & the 3 agents | Architecture graphic / Switch Console |
| **0:50 – 1:15** | Scout finds gotchas before install | Slack message & Scout's reply |
| **1:15 – 1:40** | Setup runs, hits an install error | Terminal or Switch logs showing error |
| **1:40 – 2:15** | Repair diagnoses, fixes, and retries | Repair's patch & Setup passing |
| **2:15 – 2:40** | Verification, `SETUP.md`, and wrap-up | Final `SETUP.md` & Slack completion |

---

## Spoken Script

### [0:00 – 0:30] The Problem & The Story
*(On camera or title slide)*

"Hey everyone! This is **Setup Desk**, our project for the **Flip the Switch Hackathon** in the Business Workflow track.

Every developer knows the pain of setting up someone else's open-source project. You clone the repo, run the install commands, and spend the next two hours fighting broken dependencies and outdated docs.

This project actually came straight from my own frustration during this hackathon. I was trying to run Switch on Windows, and the Antigravity integration wouldn’t work. I spent two hours debugging, only to reach out to the maintainer and find out the fix was already sitting in an open pull request!

People waste hours solving problems that the community has already solved. So I built Setup Desk to fix that."

---

### [0:30 – 0:50] The Solution & The 3 Agents
*(Show architecture graphic or Switch Console)*

"Setup Desk is an assistant that investigates open-source repositories, sets them up locally in an isolated environment, and heals itself when things fail.

You don't need a custom website—you just drop a link in Slack. Behind the scenes, **Switch** coordinates three specialized AI agents:
1. **Scout** — investigates docs and GitHub issues before running anything.
2. **Setup** — creates a virtual environment, installs packages, and tests the app.
3. **Repair** — our troubleshooter that steps in when something breaks."

---

### [0:50 – 1:15] Scout in Action
*(Show Slack channel with request and Scout's output)*

"Let’s look at a quick demo. In Slack, I ask:  
*‘Set up pallets/flask in my workspace. Check known issues first, install dependencies, run tests, and fix any safe setup errors.’*

Switch kicks off **Scout**. 

Scout is strictly read-only—it won't touch any files. It checks the repo, detects Python and `flit_core`, and notices that on Python 3.11+, an outdated pinned dev dependency has a known resolution conflict. It hands these findings directly to Setup."

---

### [1:15 – 1:40] Setup & The Failure
*(Show Setup terminal execution and error)*

"Switch hands control to **Setup**. Setup creates a clean virtual environment so it doesn't mess with your global packages.

When Setup runs `pip install`, it hits the exact dependency conflict Scout flagged. 

Normally, an automated script would just die here and leave you stranded. But Setup catches the failure, stops immediately, and hands the error logs to **Repair**."

---

### [1:40 – 2:15] Repair & The Self-Healing Loop
*(Show Repair diagnosis and Setup retry in Switch room)*

"This is Setup Desk's secret weapon: **The Self-Healing Loop**.

Repair looks at the error trace, sees the incompatible version pin, and relaxes it in `pyproject.toml`. 

It has strict safety rules: everything stays inside the workspace, no admin rights, and a hard cap of 3 tries so it never loops forever.

Repair applies the fix, explains what it changed, and hands control back to Setup. Setup re-runs the install—and it passes!"

---

### [2:15 – 2:40] Verification & Wrap-up
*(Show pytest passing, `flask --help`, and the generated `SETUP.md`)*

"Setup doesn't just install packages and assume it worked—it runs `pytest` to make sure tests pass, and launches `flask --help` to verify runtime execution.

Finally, it writes a clean `SETUP.md` file in the workspace so you have working instructions for your project, and drops a message in Slack saying you're ready to code.

With Switch coordinating the agents, setting up open-source repos goes from hours of troubleshooting to a quick message in Slack.

Thanks for watching!"
