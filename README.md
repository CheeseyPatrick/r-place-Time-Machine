# r/place Time Machine 🕰️

> ⚠️ **AI vibe coded slop.** This was generated with AI assistance. It works until it doesn't. Trust nothing, question everything. PRs welcome if it breaks.

A single-file webapp that lets you browse the [rplace.live](https://rplace.live) canvas at any point in history. Pick a date and time, and it'll find the closest git commit in the [rplacelive/canvas1](https://github.com/rplacelive/canvas1) repo and render the canvas for you — right in the browser.

## ✨ Features

- **Date + time picker** — enter any UTC datetime since 2023-06-30
- **Quick presets** — jump to notable moments with one click
- **Live rendering** — fetches the raw `place` binary and renders it pixel-by-pixel via Canvas API
- **Zoom controls** — scroll to zoom, fit-to-screen, manual ±
- **Pixel inspector** — hover to see coordinates, palette index, and hex colour
- **Prev/next navigation** — step through nearby commits
- **Download** — save the rendered canvas as a PNG
- **GitHub token support** — add a token to `localStorage.ghToken` for higher API rate limits

## 🚀 Deploying to GitHub Pages

This is a single `index.html` file — no build step, no dependencies (fonts load from Google Fonts, everything else is vanilla JS).

### Option A: direct upload

1. Fork or create a repo
2. Drop `index.html` in the root
3. Go to **Settings → Pages → Source** → `main` branch, `/ (root)`
4. Done — GitHub Pages will serve it

### Option B: clone this repo

```bash
git clone <your-fork>
cd rplace-timemachine
# edit index.html if you want
git push
```

Then enable Pages as above.

## 🔧 How it works

1. **Find the commit**: calls `GET /repos/rplacelive/canvas1/commits?until=<target>&per_page=1` to find the latest commit at or before the requested time
2. **Fetch raw files**: pulls `place` (raw binary, 1 byte per pixel) and `metadata.json` (width, height, palette) directly from `raw.githubusercontent.com` — no API quota used for the actual canvas data
3. **Render**: maps each byte to an RGBA colour using the palette, writes to a `<canvas>`, done

### Palette notes

- Commits **before** `2024-04-02T21:34:56Z` use a hardcoded 32-colour palette and a square canvas (no `metadata.json`)
- Commits **after** use `metadata.json` which contains the palette and dimensions

## ⚠️ Limitations

- GitHub unauthenticated API: **60 requests/hour**. Each "GO" click uses ~3 requests. Add a token to localStorage to get 5000/hr.
- Large canvases (2000×2000+) will take a few seconds to download
- Some very early commits may be missing or malformed
- Recording only started on **2023-06-30** — nothing before that

## 📦 Tech stack

- Vanilla HTML/CSS/JS
- Zero build tooling
- GitHub REST API + raw.githubusercontent.com
- Canvas 2D API for rendering

## 📜 Data source

Canvas data from [rplacelive/canvas1](https://github.com/rplacelive/canvas1), maintained by the [rplace.live](https://rplace.live) community.

---

*Made with vibes and caffeine. See [rplace.live](https://rplace.live) for the real thing.*
