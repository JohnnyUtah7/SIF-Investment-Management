# Install — Equity Research Studio

## A. Cursor Plugin (recommended)

Cursor discovers plugins under `~/.cursor/plugins/local/<name>/` that contain `.cursor-plugin/plugin.json`.

### Steps

1. Copy this folder:
   ```bash
   mkdir -p ~/.cursor/plugins/local
   rsync -a --delete \
     /workspace/financial-analysis/cursor-plugin/equity-research-studio/ \
     ~/.cursor/plugins/local/equity-research-studio/
   ```
   (Adjust the source path if you moved the kit.)

2. Restart Cursor **or** run `Developer: Reload Window`.

3. Open **Cursor Settings → Customize** (or Plugins) and confirm:
   - Skills: `full-company-analysis`, `sec-filings`, `dcf-model`, …
   - Agents: `research-lead`, `sec-analyst`, …
   - Rules: equity research hard rules

4. Set EDGAR identity in the shell you use for Agent terminals:
   ```bash
   echo 'export EDGAR_IDENTITY="Your Name you@email.com"' >> ~/.bashrc
   export EDGAR_IDENTITY="Your Name you@email.com"
   ```
   Optionally set the same value in the plugin’s Configure / variables UI if prompted (`EDGAR_IDENTITY`).

5. Optional tooling:
   ```bash
   pip install edgartools
   # For native Google Slides (pick one stack you already use):
   #   gws auth login
   #   or configure google-slides-skill OAuth
   ```

6. Demo:
   ```text
   Analyze AAPL end-to-end
   ```

### Zip install

```bash
cd /workspace/financial-analysis/cursor-plugin
zip -r equity-research-studio.zip equity-research-studio \
  -x '*/__pycache__/*' '*.pyc'
# Unzip into ~/.cursor/plugins/local/ on the demo machine
```

## B. Project-local skills (no plugin UI)

```bash
cd your-research-repo
mkdir -p .cursor/skills .cursor/agents
cp -R /path/to/equity-research-studio/skills/* .cursor/skills/
cp -R /path/to/equity-research-studio/agents/* .cursor/agents/
cp /path/to/equity-research-studio/rules/*.mdc .cursor/rules/ 2>/dev/null || \
  mkdir -p .cursor/rules && cp /path/to/equity-research-studio/rules/*.mdc .cursor/rules/
```

Cursor also loads `.claude/skills/` — you may copy there for dual IDE use.

## C. Claude Code marketplace-style

This folder includes `.claude-plugin/plugin.json` and root `plugin.json` for compatibility with Claude Code plugin layouts:

```text
claude plugin marketplace add <your-fork-or-path>
claude plugin install equity-research-studio@...
```

Exact marketplace commands depend on how you host the repo. For local use, copying `skills/` into `~/.claude/skills/` works.

## Post-install verification

- [ ] `~/.cursor/plugins/local/equity-research-studio/.cursor-plugin/plugin.json` exists
- [ ] Agent sees skill `full-company-analysis`
- [ ] `echo $EDGAR_IDENTITY` prints Name + email
- [ ] `python -c "import edgar"` works if you installed edgartools
- [ ] (Optional) Google Slides auth works on the demo account

## Caveats

| Topic | Note |
|-------|------|
| EDGAR 403 | Missing/invalid User-Agent identity |
| Google Slides | OAuth must be pre-authed for live USC demos; else use `slide.md` / PPTX |
| Rate limits | Pre-cache AAPL/MSFT filings before stage |
| Size | Plugin is skills/agents text only (~hundreds of KB); keep giant repos out |
| Advice | Outputs are not investment advice |
