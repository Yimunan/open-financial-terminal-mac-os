# Releasing a new version (maintainer runbook)

How to go from changed sources → refrozen `.app`/DMG → GitHub release. For a first-time/cold
build from nothing, use [`build/build-macos.sh`](build/build-macos.sh) instead (see
[build/README.md](build/README.md)); this runbook is the **incremental** path for an existing
build tree (the script's `ROOT` with `.venv-desktop`, `open-financial-terminal/`,
`quant-hedge-fund-incubator/` as siblings).

## 0. What kind of change is it?

| Changed | Rebuild needed |
|---|---|
| `backend/**` or `qhfi` (`src/qhfi/**`) Python | refreeze only (PyInstaller reads source live) |
| `frontend/src/**` | `npm run build` first, **then** refreeze (the spec bundles `frontend/dist`) |
| `build/oft-macos.spec`, `build/rthook_env.py` | copy into `backend/` first (the freeze reads the copies), then refreeze |
| docs/screenshots only | no rebuild — commit/push this repo only |

## 1. Rebuild

```sh
ROOT=~/oft-build                                 # your build tree (siblings layout)
cd $ROOT/open-financial-terminal

# frontend changed? rebuild the SPA the spec will bundle
( cd frontend && npm run build )

# freeze the .app (steps 5–6 of build-macos.sh)
( cd backend && $ROOT/.venv-desktop/bin/pyinstaller oft-macos.spec --noconfirm \
    --distpath dist-mac --workpath build-mac )
APP="backend/dist-mac/Open Financial Terminal.app"
codesign --force --deep --sign - "$APP"
codesign --verify --deep --strict "$APP"

# package the DMG
STAGE="$(mktemp -d)/OFT"; mkdir -p "$STAGE"
cp -R "$APP" "$STAGE/"; ln -s /Applications "$STAGE/Applications"
hdiutil create -volname "Open Financial Terminal" -srcfolder "$STAGE" \
  -ov -format UDZO "$ROOT/OpenFinancialTerminal.dmg"
```

## 2. Verify before shipping

Install the fresh build locally (`rm -rf "/Applications/Open Financial Terminal.app" && cp -R
"$APP" /Applications/ && open "/Applications/Open Financial Terminal.app"`), then:

- [ ] backend tests pass: `cd backend && $ROOT/.venv-desktop/bin/python -m pytest tests/ -q`
- [ ] app launches; `curl http://127.0.0.1:8050/api/health` → `"status":"ok"`
- [ ] **filings live** (regression guard for the SEC UA/latch bugs):
      `curl "http://127.0.0.1:8050/api/filings?symbol=TSLA"` → `"coverage":"live"` with items
- [ ] **macro refresh** honest: `curl -X POST http://127.0.0.1:8050/api/settings/data-refresh/macro/run`
      → `"status":"ok"` with `series > 0` (a 0-series batch must report `error`)
- [ ] **risk attribution** fits: `curl -X POST http://127.0.0.1:8050/api/risk/attribution
      -H 'Content-Type: application/json' -d '{"source":"holdings"}'` → factor/position payload
      (needs some holdings saved)
- [ ] LLM round-trip if configured: `curl -X POST http://127.0.0.1:8050/api/settings/llm/probe -d '{}'`
- [ ] eyeball the workspace: chart renders, ⌘K opens, Settings chip reads Local API / Online API
      correctly for the configured base URL

## 3. Cut the release

The DMG (~240 MB) exceeds GitHub's 100 MB file limit — it ships as a **release asset**, never in
git (`.gitignore` already excludes `*.dmg`). The README's Install button points at
`releases/latest`, so publishing automatically updates it.

```sh
# commit + push whatever changed in THIS repo first (README, build/, docs/screenshots/)
git push origin main

gh -R Yimunan/open-financial-terminal-mac-os release create vX.Y.Z \
  "$ROOT/OpenFinancialTerminal.dmg" \
  --target main \
  --title "Open Financial Terminal vX.Y.Z (macOS, Apple Silicon)" \
  --notes "…what changed + the standard install/Gatekeeper blurb (copy a previous release)…"
```

Versioning so far: patch bump per shipped fix batch (v1.0.1 rebuild, v1.0.2 UI fix, v1.0.3 data-layer
fixes). Verify after upload:

```sh
gh -R Yimunan/open-financial-terminal-mac-os release view --json tagName --jq .tagName   # → vX.Y.Z (latest)
gh -R Yimunan/open-financial-terminal-mac-os release view vX.Y.Z \
  --json assets --jq '.assets[]|"\(.name) \(.size) \(.state)"'                           # → uploaded
```

## Gotchas learned the hard way

- **`SEC_USER_AGENT` must contain a contact email** (`build/rthook_env.py`) — EDGAR 403s UAs
  without one, which silently breaks all filings in packaged builds (dev runs mask it).
- The spec bundles `frontend/dist` **as it exists on disk** — a stale dist ships stale UI with no
  error. When in doubt, rebuild the SPA.
- Keep the same DMG asset name (`OpenFinancialTerminal.dmg`) across releases so old links break
  loudly (404 to the tag) rather than silently serving a mismatched file.
- Ad-hoc signing only — first launch needs the Gatekeeper dance in INSTALL-macOS.md. Real
  notarization requires an Apple Developer account.
