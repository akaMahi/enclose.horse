# HTML5 build — GitHub Pages

This folder holds everything needed to run the game on GitHub Pages.

## What's here

| File | Purpose |
| ---- | ------- |
| `index.html` + `index.js` + `index.wasm` + `index.pck` (+ worklets/icons) | Exported Godot Web build (Godot 4.7, `Web` preset, thread support **off**) |
| `.nojekyll` | Tells Pages to serve files as-is (no Jekyll processing) |
| `workflows/godot-web.yml` | CI workflow that rebuilds + deploys on every push to `main` |
| `README.md` | This file |

No `coi-serviceworker.js` hack is needed: the `Web` preset in
`../export_presets.cfg` sets `variant/thread_support=false`, so the game
runs on Pages without cross-origin isolation headers.

## Option A — deploy this exact build (manual)

1. Push this folder's **contents** to the root of a `gh-pages` branch
   (or to the root of a `<user>.github.io` repo).
2. Repo Settings -> Pages -> Deploy from a branch -> `gh-pages` / root.
3. Open the Pages URL — the game loads `index.html` automatically.

## Option B — auto-rebuild on every push (recommended)

1. Copy `workflows/godot-web.yml` to `.github/workflows/godot-web.yml`
   at the repo root (GitHub only runs workflows from `.github/workflows/`).
2. Repo Settings -> Pages -> Build and deployment -> Source: **GitHub Actions**.
3. Push to `main`. The workflow downloads Godot 4.7 + templates,
   re-exports the `Web` preset into `github/`, and publishes it.

If your Godot project lives in a subfolder instead of the repo root,
set `PROJECT_DIR` in the workflow (e.g. `game`).

## Re-exporting locally

Godot binary and export templates must match **exactly**:

```sh
# binary 4.5.1 + templates 4.5.1.stable
Godot --headless --path /path/to/intro --export-release Web /path/to/intro/github/index.html
```

The checked-in `../export_presets.cfg` already defines the `Web` preset
(export path `github/index.html`, no threads).
