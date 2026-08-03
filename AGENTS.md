# AGENTS.md

## Project overview

This repository is a **static single-page app** (no backend, no package manager). The canonical app file is `CHREZENT_ClaudeDoc_20260510_173933.html` — a client-side Goshiwon (boarding house) management tool that persists data in browser `localStorage`.

Other HTML files in the root are older snapshots or mockups; use the canonical file unless a task specifies otherwise.

## Cursor Cloud specific instructions

### Services

| Service | Required | Notes |
|---------|----------|-------|
| Static HTTP server | Yes (recommended) | Serve from repo root; avoid `file://` for CDN scripts and uploads |
| Browser | Yes | Chrome/Firefox/Edge |
| CDN internet access | Yes | SheetJS, Pretendard, Google Fonts load from external URLs |
| Backend / DB / Docker | No | Not part of this repo |

### Running the app

From the repository root:

```bash
python3 -m http.server 8080
```

Open: `http://localhost:8080/CHREZENT_ClaudeDoc_20260510_173933.html`

Default login password (hardcoded in the HTML): `0000`

Use a tmux session for long-running servers (see cloud agent shell guidelines).

### Lint / test / build

There is **no** npm/pip dependency tree, CI config, or automated test suite in this repo.

For smoke checks after edits:

- Confirm the page returns HTTP 200 via curl
- Manually log in and exercise the changed panel in the browser
- After JS patches, follow the embedded build manual in the HTML: run `node --check` on extracted script if applicable

Optional dev tools already available on the VM: Python 3, Node.js (for `node --check` validation only).

### Patching large HTML files

The app embeds a build manual inside `CHREZENT_ClaudeDoc_20260510_173933.html`. Prefer atomic writes (`os.replace`) when patching with Python; validate with `node --check` or load tests before reporting completion.

### Secrets and data sensitivity

- No `.env` or API keys are required for core flows
- HTML embeds real tenant/financial sample data — treat backups and exports as sensitive
- Login images (`ukuk_1.jfif`, `ukuk_2.jfif`) are referenced but not shipped in the repo (cosmetic only)
