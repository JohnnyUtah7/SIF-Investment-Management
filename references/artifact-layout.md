# Artifact layout

All run outputs for ticker `TICKER` land under:

```
artifacts/{TICKER}/
  00-intake.md                 # company identity, CIK, peers seed
  01-sec/
    filings_index.json         # forms, dates, accessions
    financials.json            # IS/BS/CF extracted
    notes.md                   # MD&A / risk excerpts citations
    dilution_if_converted.md   # optional; from dilution-if-converted skill
  02-statements/
    normalized_is_bs_cf.md
    fcf_bridge.md
  03-models/
    dcf.md | dcf.xlsx
    comps.md | comps.xlsx
    lbo.md | lbo.xlsx
    sotp.md
    sensitivity.md
  04-research/
    competitive.md
    unit_economics.md
    catalysts.md
    risk_audit.md
    memo.md                    # OFFICIAL equity research memo (full body)
  05-deck/
    slide.md                   # REQUIRED markdown slide source
    deck.pptx                  # optional PPTX or symlink
    deck_link.txt              # Google Slides URL or PPTX path
  RUNLOG.md                    # orchestrator checklist + timestamps
```

## Path discipline (acceptance)

| Artifact | Official path | Rule |
|----------|---------------|------|
| Equity research memo | `04-research/memo.md` | **Full memo body** (not a pointer/stub). Studio acceptance requires this file. A house/docs copy elsewhere is optional and is a *copy* only. |
| Deck | `05-deck/slide.md` at minimum | If PPTX or Google Slides is produced, also place or symlink under `05-deck/` (e.g. `deck.pptx`) and record the path/URL in RUNLOG + `deck_link.txt`. |
| Dilution extract | `01-sec/dilution_if_converted.md` | When convertibles/ATM/options exist. |

### RUNLOG completion gates
- **Cannot** mark `equity-research-memo` complete if `04-research/memo.md` is missing, is a pointer-only stub, or is under ~30 lines of substantive body.
- **Cannot** mark `slides-deck` complete if `05-deck/` is empty or lacks `slide.md`.

Create missing folders as you go. Never overwrite without updating RUNLOG.
