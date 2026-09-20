# Codebase Viewer — local

Repo: https://github.com/sturhan2a/codebase-viewer-local

Offline-capable copy of [codebaseviewer.vercel.app](https://codebaseviewer.vercel.app).

The original site is a static UI on Vercel talking to a remote indexer (`https://cp1.tailaea96f.ts.net`). This package keeps that UI and replaces the remote indexer with a same-origin Node server. **Nothing is fetched from Vercel, Tailscale, or GitHub while it runs.**

## Requirements

- Node.js 18+ (stdlib only — no `npm install`)

## Run

```bash
git clone https://github.com/sturhan2a/codebase-viewer-local.git
cd codebase-viewer-local
node server.js
```

Then open http://127.0.0.1:8787

Optional:

```bash
PORT=9000 HOST=127.0.0.1 node server.js
```

## Use

1. Click the folder button.
2. Choose a local project directory (File System Access API) or use the fallback file picker.
3. Wait for indexing. The 2D / 3D landscape, search, inspector, and source preview all run against the local indexer.

GitHub URL import is **disabled** on purpose. Clone the repo yourself, then open the folder.

## What stays local

| Piece | Where it lives |
| --- | --- |
| UI (`public/`) | Served from this folder |
| Uploaded files | OS temp dir (`codebase-viewer-local` under your temp folder) |
| Search / symbols / edges | In-memory in `server.js` |
| Preferences | Browser `localStorage` only |

Skipped during walk/upload: `.git`, `node_modules`, `target`, `dist`, `build`, and common binary types. Individual files over 8 MB are skipped (same as the original client).

## Notes

- Open via the local server, not as `file://`. ES modules and `/api` need HTTP.
- Bind is `127.0.0.1` by default so the indexer is not exposed on the LAN.
- Symbol and import extraction is regex-based, not a full language server. It is good enough for the landscape and search; it will miss some edges the hosted indexer might catch.
- This is an independent local reconstruction of the public frontend + API the site uses. It is not the original private backend source.
