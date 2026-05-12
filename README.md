# mcp-host

Public distribution channel for the **xTrade MCP** macOS menu-bar
app. The host polls `latest.json` here to discover new versions
and downloads the DMG attached to the matching GitHub Release.

## Files

- **`latest.json`** — version manifest. Plain JSON, `application/json`.
  Polled by every running install on a weekly cadence (configurable
  via Preferences in the app). Schema:

  ```json
  {
    "version": "1.1.3",
    "url": "https://github.com/lukataylo/mcp-host/releases/download/v1.1.3/xTrade.MCP.dmg",
    "notes": "Short release-notes blurb shown in the tray tooltip.",
    "minSystemVersion": "14.0"
  }
  ```

  - `version`: SemVer. Compared against the running app's
    `CFBundleShortVersionString`. Newer → "Update available".
  - `url`: direct DMG download. Must be anonymously reachable.
  - `notes`: optional. Shown as the menu tooltip.
  - `minSystemVersion`: optional. Reserved; currently informational.

- **`releases/`** — each tag (`v1.1.3`, etc.) is a GitHub Release
  on this repo with the DMG attached. The DMG is the same binary
  cut from the main `xtrade-mcp` repo's release workflow, mirrored
  here so anyone can download without GitHub auth.

## How updates reach end users

1. The running app polls
   `https://raw.githubusercontent.com/lukataylo/mcp-host/main/latest.json`
   weekly (or on launch, depending on the user's setting).
2. If the JSON's `version` is newer than the running build, the
   menu shows **Update available: vX.Y.Z**.
3. Clicking it opens the DMG URL in the user's browser.

The xTrade MCP source code stays in the private
[xtrade-mcp](https://github.com/lukataylo/xtrade-mcp) repo. This
repo only holds the artefacts users need to download — version
metadata and the DMG itself.

## Cutting a release

The main repo's `release.yml` workflow builds the DMG and creates
a release on `xtrade-mcp` (private). A follow-on step mirrors the
DMG here and bumps `latest.json`. If automation breaks:

```sh
gh release create v1.1.3 \
  --repo lukataylo/mcp-host \
  --title "v1.1.3 — xTrade MCP" \
  --notes-file notes.md \
  /path/to/xTrade.MCP.dmg
# then update latest.json + git push
```

See the [Releases wiki page](https://github.com/lukataylo/xtrade-mcp/wiki/Releases)
in the main repo for the full process.
