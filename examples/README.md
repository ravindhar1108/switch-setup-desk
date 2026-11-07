# Setup Desk Examples

These files show what a full Setup Desk run looks like using [Flask (`pallets/flask`)](https://github.com/pallets/flask) as an example.

*(Note: These are reference examples showing how the agents talk to each other, not physical live terminal dumps.)*

---

## The Scenario

1. [**Sample Slack Request (`sample-request.md`)**](sample-request.md)  
   What the user types in Slack to trigger the setup.
2. [**Scout Report Example (`scout-report.md`)**](scout-report.md)  
   What Scout discovers before running any commands.
3. [**Setup Execution Report (`setup-report.md`)**](setup-report.md)  
   Setup creating the `.venv`, hitting a version error, and retrying after repair.
4. [**Repair Report Example (`repair-report.md`)**](repair-report.md)  
   How Repair reads the error, patches the version constraint, and hands back to Setup.
