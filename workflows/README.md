# Setup Desk Workflows

Setup Desk has two main flows:

1. [**Standard Setup (`setup-workflow.md`)**](setup-workflow.md)  
   The clean path: Scout checks the repo $\rightarrow$ Setup installs packages and verifies the app $\rightarrow$ You get a verified `SETUP.md` in Slack.

2. [**Self-Healing Recovery (`self-healing-workflow.md`)**](self-healing-workflow.md)  
   The recovery path: Setup hits an error $\rightarrow$ Repair diagnoses and fixes it $\rightarrow$ Setup retries (up to 3 times) $\rightarrow$ Verifies and wraps up.

---

## Quick Comparison

| Flow | What triggers it | Agents involved | Retries |
|---|---|---|---|
| **Standard** | Regular Slack request | Scout $\rightarrow$ Setup | 0 |
| **Self-Healing** | Install or test error | Scout $\rightarrow$ Setup $\rightleftharpoons$ Repair | Up to 3 |
