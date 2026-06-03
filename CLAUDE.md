# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
bundle exec jekyll serve   # local dev server
bundle exec jekyll build   # production build
npx prettier --write .     # format all files
```

## Architecture

Jekyll static site for Procedural — a gallery specialising in procedurally generated art. It deploys to GitHub Pages on push to `main` via `.github/workflows/main.yml`.

**Collections** (`_config.yml`):
- `_artists/` — one `.md` per artist, front matter: `name`, `layout: artist`
- `_artworks/` — one `.md` per artwork, front matter: `layout: artwork`, `artist: <slug>`, `title`, `medium`, `dimensions`, `p5: true|false`
- `_exhibitions/` — one `.md` per exhibition, front matter: `layout: exhibition`, `title`, `start_date`, `end_date`, `artworks: [<slug>, ...]`

**Artwork rendering** — controlled by the `p5` front matter field:
- `p5: true` → loads p5.js + `assets/js/2d.js`. Each artwork's `<script>` defines `WIDTH`, `HEIGHT`, `SCALE`, `RENDERER`, `SEED` constants and a `sketch()` function. The shared `2d.js` handles canvas setup, seeding, scaling, and the Generate/Share buttons.
- `p5: false` → loads Three.js via importmap + `assets/js/3d.js` as an ES module. The `3d.js` module exports helpers (`init`, `loadAssets`, `setupGallery`, `scaleCanvas`, `setupControls`) that artwork scripts import to build a 3D scene.

Both rendering modes support seed-based generation and URL sharing (via `?seed=<n>`).

**Styling**: Tailwind CSS v3 processed by `jekyll-postcss`. Class sorting enforced by Prettier with `prettier-plugin-tailwindcss`. Dark mode is supported throughout (uses `dark:` variants). The `safelist` in `tailwind.config.js` includes `block/hidden/dark:block/dark:hidden` for JS-toggled visibility.

**UI components**: Uses `@tailwindplus/elements` (CDN) for `<el-dropdown>` / `<el-menu>` in the header.

**Sibling sites**: The header nav links to three related sites — Acta Machina, hyperfocal, and Procedural — defined under `sites:` in `_config.yml`. The logo switcher in the header uses a shared SVG sprite at `/assets/images/shared/logos.svg`.

**Artist–Artwork–Exhibition relationships**: Artworks reference their artist by slug (`artist: charles-lorent`). The artist layout queries `site.artworks | where: "artist", page.slug` and derives related exhibitions by checking if any artwork slug appears in `exhibition.artworks`. Exhibitions reference artworks by slug in their front matter array.
