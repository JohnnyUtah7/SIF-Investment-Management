---
name: analyze-company
description: Run full Equity Research Studio pipeline for a ticker (orchestrator)
---

# Analyze company

Run skill `full-company-analysis` for the ticker provided in the argument or chat.

1. Confirm ticker
2. Ensure `EDGAR_IDENTITY` is set (prompt if not)
3. Execute orchestrator phases 1→14
4. Return paths to memo and deck
