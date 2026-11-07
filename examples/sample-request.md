# Sample Slack Request

Here is what triggering Setup Desk looks like in Slack:

---

**Channel:** `#setup-desk`  
**User:** `@alex`  

### The Message:

> `@SetupDesk Set up https://github.com/pallets/flask in my workspace. Check known issues first, install dependencies, run tests, and fix any safe setup errors.`

---

### What happens in the thread:

1. **Bot pings back:**  
   > *"Got it! Launching Scout to inspect `pallets/flask`..."*
2. **Scout posts findings:**  
   > *"Detected Python project with `flit_core`. Warning: On Python 3.11+, dev dependency `x` has a known version conflict. Handing to Setup."*
3. **Setup clones & installs:**  
   > *"Created `.venv`. Running `pip install -e .[dev]`..."*
4. **Failure & Repair intervention:**  
   > *"Install failed with dependency conflict. Repair is stepping in (Attempt 1 of 3)..."*  
   > *"Repair relaxed pinned constraint in `pyproject.toml`. Retrying install..."*
5. **Setup verifies & finishes:**  
   > *"Install succeeded! Running `pytest`... all tests passed.*  
   > *Verified runtime with `flask --help`.*  
   > *Created `SETUP.md`. You're ready to code!"*
