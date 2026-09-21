# SIF Investment Management

Public Cursor/Claude plugin for **SIF Investment Management** (USC Marshall) — equity research workflow:

**SEC filings → statements → DCF / comps / LBO / SOTP → memo → Google Slides-ready deck**

One prompt runs the pipeline:

```text
Analyze AAPL end-to-end
```
```text
check out this company: NVDA
```

## What’s inside

| Component | Count | Role |
|-----------|------:|------|
| Skills (`skills/*/SKILL.md`) | 16 | Including orchestrator `full-company-analysis` + thin `dilution-if-converted` |
| Agents (`agents/*.md`) | 8 | Research Lead → Deck Producer |
| Commands | 4 | `/analyze-company`, `/dcf`, `/sec-pull`, `/slides` |
| Rules | 1 | Never invent SEC numbers; dilution/ARR/mix/path gates |
| Scripts | 1 | Thin `edgar_pull.py` wrapper (optional) |

**Not included:** multi-GB clones of edgartools / printing-press / etc. Install those tools separately if you want live EDGAR pulls.

## Install (Cursor)

**Recommended — local plugin folder:**

```bash
mkdir -p ~/.cursor/plugins/local
cp -R /path/to/equity-research-studio ~/.cursor/plugins/local/
# Restart Cursor or Developer: Reload Window
```

Then open **Customize** and confirm skills/agents from `equity-research-studio`.

**Alternate — project skills only:**

```bash
mkdir -p .cursor/skills
cp -R skills/* .cursor/skills/
mkdir -p .cursor/agents
cp -R agents/* .cursor/agents/
```

See **INSTALL.md** for step-by-step and Claude Code notes.

## First run checklist

1. **SEC identity** (required for EDGAR):
   ```bash
   export EDGAR_IDENTITY="Chris Miller chris@example.com"
   pip install edgartools   # optional but recommended
   # If FileStorage/hishel errors: pip install 'hishel==0.1.3'  (1.1.8 broke)
   ```
2. In Cursor Agent chat:
   ```text
   Analyze AAPL end-to-end
   ```
3. Artifacts land in `artifacts/AAPL/` — official memo at `04-research/memo.md`, deck at `05-deck/slide.md`.

## Google Slides

Native Slides needs OAuth (`gws auth login` or google-slides-skill credentials) **before** a live demo. Without it, the plugin still writes `slide.md` (and can fall back to PPTX → Drive → Open with Google Slides). PPTX/links must still land under `05-deck/`.

## Layout

```text
equity-research-studio/
├── .cursor-plugin/plugin.json    # Cursor Plugin manifest
├── .claude-plugin/plugin.json    # Claude Code-friendly twin
├── .claude-plugin/marketplace.json
├── plugin.json                   # Portable Agent Plugin manifest
├── skills/                       # 16 skills
├── agents/                       # 8 agents
├── commands/
├── rules/
├── references/
├── scripts/edgar_pull.py
├── examples/prompts.md
├── artifacts/.gitkeep
├── INSTALL.md
└── README.md
```

## CHANGELOG

### 1.1.0 — 2026-09-20 (PT) — King'sHand IREN audit fixes

Hard gates from IREN FAIL/PARTIAL audit:

| Area | Before (IREN-class failure) | After (1.1.0) |
|------|----------------------------|---------------|
| **Path / memo** | Memo could live only under house/docs; `04-research/memo.md` pointer/stub; RUNLOG still “complete” | Official full body **must** be `artifacts/{TICKER}/04-research/memo.md`; copies optional; RUNLOG blocked if pointer-only / &lt;~30 lines |
| **Path / deck** | Slides marked done with empty `05-deck/` | Must write `05-deck/slide.md`; PPTX/Slides under `05-deck/` + RUNLOG path; empty folder → incomplete |
| **Dilution** | Undiluted $/sh presented as “the” value despite converts/ATM | Basic **vs fully diluted** required; new thin skill `dilution-if-converted`; DATA_GAP on if-converted → no undiluted-as-the-value |
| **ARR / non-GAAP** | ARR juxtaposed with GAAP revenue; circular tape multiples unmarked | ARR always labeled **non-GAAP operating metric**; SOTP/comps must disclose **tape-implied / circular** ARR multiples |
| **Mix transition** | Pure-AI multiples on BTC-heavy LTM | Explicit **mix transition** callout required in comps/competitive/memo/slides |
| **TV visibility** | TV as % of EV easy to omit | Required in DCF output + DCF slide map |
| **EDGAR pin** | hishel 1.1.8 FileStorage break undocumented | Pin note: `hishel==0.1.3` in sec-filings, edgar-identity, README |
| **Risk audit** | No mandatory re-derive sample | Required re-derive table + IREN-class gate checklist |
| **Orchestrator** | Thin acceptance list | Expanded checklist covering all gates above |

Also: skill count 15 → 16; manifests bumped to **1.1.0**.

### 1.0.1 — 2026-09-20 (PT)

Post-pack audit: author email + marketplace fix.

### 1.0.0

Initial lean studio pack.

## Provenance

Techniques distilled from open sources documented in `references/sources.md` (edgartools, analyst-kit, Anthropic financial-services, equity-research-skill, slides skills). Curated text only — not a vendored mega-repo.

## Disclaimer

Educational / professional workflow aid. **Not investment advice.**
