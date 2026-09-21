---
name: dilution-if-converted
description: >
  Extract convertible notes, ATM programs, options/RSUs/warrants and compute
  basic vs if-converted / fully diluted share counts from 10-K notes (e.g. Note
  pattern like IREN Note 23). Use before per-share DCF/comps/SOTP/memo when
  convertibles or equity overhang may exist.
version: "1.1.0"
---

# Dilution / If-Converted Share Count

<!-- Thin skill. Inspired by IREN-class 10-K convertible note disclosures
     (e.g. Note 23 pattern): ITM converts, conversion rates, capped calls, ATM. -->

## Hard rules
- **Never invent** conversion rates, principal, or share counts — cite form/note/date.
- Always present **basic** and **fully diluted / if-converted** side by side.
- If if-converted shares cannot be sourced → `DATA_GAP`. Downstream skills must
  **not** present undiluted $/sh as “the” equity value.
- Flag ITM vs OTM converts; do not silently assume all converts dilute.

## When to run
- After `sec-filings` / alongside `financial-statements`, before DCF/comps/SOTP per-share.
- Any mention of convertible notes, convertible senior notes, ATM, equity line,
  warrants, large RSU overhang, or “if-converted” in filings.

## Extract checklist (from 10-K/10-Q notes + diluted EPS footnote)
1. **Basic shares** — weighted-average and period-end common shares outstanding (cite).
2. **Diluted W.A. shares** — from EPS note (cite).
3. **Convertibles** — principal, coupon, maturity, conversion price/rate, current ITM/OTM vs spot, capped-call or bond-hedge if any.
4. **If-converted shares** — principal ÷ conversion price (or disclosed conversion rate × principal); show math.
5. **ATM / equity programs** — capacity remaining, shares issued YTD (cite).
6. **Options / RSUs / warrants** — dilutive potential if disclosed; else DATA_GAP with note.
7. **Fully diluted estimate** — basic + ITM converts + dilutive options (state method: treasury stock vs if-converted); list exclusions.

## Output table (minimum)
| Item | Shares (mm) | Source |
|------|------------:|--------|
| Basic (period-end) | | 10-K/10-Q |
| Diluted W.A. (EPS) | | EPS note |
| If-converted (ITM converts) | | Note X |
| ATM capacity (shares, est.) | | Note / shelf |
| FD estimate (method labeled) | | bridge |
| DATA_GAPs | | |

## Outputs
```
artifacts/{TICKER}/01-sec/dilution_if_converted.md
```

## Handoff
DCF, comps, SOTP, memo, slides, and risk-audit **must** consume this artifact when present and label $/sh as basic vs FD.
