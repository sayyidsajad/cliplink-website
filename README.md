# ClipLink website

The download **and transparency** hub for [ClipLink](https://github.com/sayyidsajad/cliplink)
— a macOS menu-bar app that turns Finder `⌘C` into a private, expiring share link.

**Live site:** <https://sayyidsajad.github.io/cliplink-website/>

[![build](https://img.shields.io/github/actions/workflow/status/sayyidsajad/cliplink/ci.yml?branch=main&label=build)][ci]
[![release](https://img.shields.io/github/v/release/sayyidsajad/cliplink)][releases]
[![license](https://img.shields.io/github/license/sayyidsajad/cliplink)][license]

## What lives here

This repo is intentionally **one static page and an icon** — no build step, no
analytics, no trackers, no third-party JavaScript.

```
index.html     the whole site (inline CSS/JS, single file)
favicon.svg    simple inline SVG
```

## How the download button works

The **Download** button uses a tiny client-side script (inline, inspectable)
that calls GitHub's public API:

```
https://api.github.com/repos/sayyidsajad/cliplink/releases/latest
```

and points the visitor at the `.dmg` asset of whatever release is newest,
whatever the version. If the API call fails, the button falls back to the
releases page — the download can never be broken, and **no credentials,
tracking or cookies** are involved.

## Deployment

GitHub Pages builds `main` automatically on every push
(Settings → Pages → deploy from branch `main`). Nothing to run, nothing to
configure.

### Local preview

```bash
python3 -m http.server 8080      # from this directory
# → http://localhost:8080
```

## Where is everything else?

| Piece | Where | Notes |
|---|---|---|
| Mac app + CLI + core library | [`sayyidsajad/cliplink` → `macapp/`](https://github.com/sayyidsajad/cliplink/tree/main/macapp) | Swift, builds with `swift build` |
| Backend (API) | [`sayyidsajad/cliplink` → `backend/`](https://github.com/sayyidsajad/cliplink/tree/main/backend) | Cloudflare Worker + D1 + Supabase Storage, ~2 JS files |
| Release pipeline | [`ci.yml` / `release.yml` workflows][ci] | DMG is compiled by GitHub's macOS runners on every `v*` tag and attached to the release |
| Backend URL | `https://api.cliplink.workers.dev` | Preconfigured inside released DMGs; full re-deploy docs in the [main README][readme] |
| Security policy | [`SECURITY.md`](https://github.com/sayyidsajad/cliplink/blob/main/SECURITY.md) | How to report a vulnerability |

## Maintenance guide

- **Support links** (GitHub Sponsors, Buy Me a Coffee) live in the
  `#support` section of `index.html` — plain `<a href>`s.
- **Verified outbound links** (checked 2026-10-24):
  - `https://sayyidsajad.github.io/cliplink-website/` — this site, HTTP 200
  - `https://github.com/sayyidsajad/cliplink` — main repo
  - `https://github.com/sayyidsajad/cliplink/releases/latest` — latest DMG
  - `https://github.com/sponsors/sayyidsajad` — HTTP 200 (profile live)
  - `https://www.buymeacoffee.com/sayyidsajad` — HTTP 200 (profile live)
  - `https://api.cliplink.workers.dev/healthz` — backend liveness

## Trust notes

- The site itself sets a **strict CSP** meta policy: scripts/styles inline-only,
  external connections restricted to `api.github.com`, no frames, no forms.
- No cookies, no fingerprinting, no ad scripts, no external fonts.
- The main project's license is **MIT** ([text][license]).

[ci]: https://github.com/sayyidsajad/cliplink/actions
[releases]: https://github.com/sayyidsajad/cliplink/releases
[license]: https://github.com/sayyidsajad/cliplink/blob/main/LICENSE
[readme]: https://github.com/sayyidsajad/cliplink#readme
