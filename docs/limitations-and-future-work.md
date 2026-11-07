# Limitations & Future Ideas

Being honest about what works and what doesn't is important for any hackathon project.

---

## Current Limitations

- **Slack required:** Right now, you interact with Setup Desk through Slack. If you don't use Slack, you can't easily trigger the workflow.
- **No system packages:** Because agents run without sudo/admin, they can't install things like `gcc`, `cmake`, or system databases (`postgresql`). If a project requires those and they aren't on your machine, Setup Desk stops and alerts you.
- **Max 3 repair tries:** If a project has deep code bugs or requires extensive refactoring, Setup Desk will hit its 3-attempt limit and hand control back to you.
- **Tested mainly on Python & Node:** We've tested this workflow primarily on Python and Node.js projects. Complex monorepos or C/C++ builds haven't been heavily tested yet.

---

## Future Ideas (Not Yet Implemented)

- **Docker sandboxing:** Run the setup inside disposable containers so agents can install system-level packages safely.
- **Support more languages:** Add dedicated rules for Rust (`cargo`), Go (`go mod`), and monorepos.
- **Setup Scorecard:** Have Scout output a 1–10 "Setup Health Score" before running anything, warning you if a repo's setup is broken.
- **Shared Setup Cache:** Save verified `SETUP.md` guides to a public cache so the next person setting up the same repo gets verified steps instantly.
- **CLI / GitHub Action:** Run Setup Desk directly from your terminal or as a PR check in CI.
