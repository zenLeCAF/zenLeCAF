# AGENTS.md

## Project overview

**청화(淸和) Regent / CHREZENT** is a client-side goshiwon (고시원) management SPA shipped as self-contained HTML files. There is no build step, package manager, or backend server in this repository.

- **Primary app file:** `CHREZENT_ClaudeDoc_20260510_173933.html` (latest full build)
- **Data storage:** browser `localStorage` (export/import via JSON backup in the UI)
- **Default login password:** `0000` (client-side gate only; not secure for production)

## Development commands

This repo has no npm/pip dependencies, lint config, or automated tests.

| Action | Command |
|--------|---------|
| Serve locally | `python3 -m http.server 8080 --bind 127.0.0.1` (from repo root) |
| Open app | `http://127.0.0.1:8080/CHREZENT_ClaudeDoc_20260510_173933.html` |

Use an HTTP server rather than opening the HTML via `file://` so CDN scripts (SheetJS, fonts) and file uploads behave consistently.

## Cursor Cloud specific instructions

### Services

Only one process is required for local development:

| Service | Required | How to start |
|---------|----------|--------------|
| Static HTTP server | Yes | `python3 -m http.server 8080 --bind 127.0.0.1` in `/workspace` |
| Browser (Chrome) | Yes | Open the URL above |
| Backend / database / Docker | No | All persistence is `localStorage` |

Run the static server in a **tmux** session so it stays up across agent steps:

```bash
SESSION_NAME="chrezent-static-server"
tmux -f /exec-daemon/tmux.portal.conf has-session -t "=$SESSION_NAME" 2>/dev/null \
  || tmux -f /exec-daemon/tmux.portal.conf new-session -d -s "$SESSION_NAME" -c "/workspace" \
     -- "${SHELL:-bash}" -l -c "python3 -m http.server 8080 --bind 127.0.0.1"
```

Verify with: `curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8080/CHREZENT_ClaudeDoc_20260510_173933.html` (expect `200`).

### Hello-world smoke test

1. Start the static server (see above).
2. Open the app in Chrome and log in with password `0000`.
3. Confirm the room dashboard and navigation bar load (e.g. 입실관리, 월별입금).
4. Click a room (e.g. 101) and confirm the detail side panel opens.

### Gotchas

- **No lint or test suite** — there is nothing to run beyond serving the HTML and manual/browser verification.
- **CDN dependencies** — SheetJS and web fonts load from `cdn.jsdelivr.net` / Google Fonts; offline mode limits Excel import/export and typography.
- **Missing login images** — `ukuk_1.jfif` and `ukuk_2.jfif` are referenced but not in the repo; login still works.
- **SMS/Kakao/QR** — messaging features open device SMS links or copy to clipboard; they are optional for core management flows.
- **Data is per-browser** — use **전체 백업** / **백업 복원** in the UI to move data between machines.
