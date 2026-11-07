# Why We Built It This Way

Here are the key design choices behind Setup Desk and why we made them:

---

### 1. Why 3 agents instead of 1 giant prompt?
A single agent with one huge prompt tries to do everything at once. It reads a 500-line README, searches GitHub, runs terminal commands, gets confused, fills up its context window, and hallucinates.  
By splitting the work:
- **Scout** only reads and searches.
- **Setup** only runs verified commands.
- **Repair** only diagnoses errors.

### 2. Why check GitHub issues/PRs before installing?
Documentation is almost always outdated. The fix for a Python 3.11 build failure or Windows path issue is usually already solved in an open pull request. Scout looks for that first so we don't hit obvious walls.

### 3. Why Slack instead of a web dashboard?
Nobody needs another dashboard or web portal. Developers already live in Slack. Submitting a link in a channel and getting a notification when it's ready fits naturally into how teams work.

### 4. Why a separate Repair agent?
When code fails, the agent that ran it often makes hasty guesses or repeats the same error. A dedicated Repair agent takes a step back, inspects the error trace objectively, applies a minimal fix, and hands it back to Setup to test.

### 5. Why cap retries at 3?
Infinite agent loops waste tokens and make things worse. If an issue can't be resolved in 3 tries, it's usually a missing system library (like a C++ compiler) that needs human decision-making.

### 6. Why generate `SETUP.md`?
Once the agent finishes, you shouldn't have to re-run the agent next time. `SETUP.md` leaves you with verified, working instructions for your project.

### 7. Why strict workspace boundaries?
An AI agent should never have root access or touch files outside the project folder. Keeping everything inside a local `.venv` ensures your machine stays clean and safe.
