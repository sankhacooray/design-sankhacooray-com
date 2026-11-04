# design

**[design.sankhacooray.com](https://design.sankhacooray.com)** — the visual & interaction design hub in the
sankhacooray.com network: the PayNow deep-link sandbox, meta share-card gallery, Pixels image search,
Helix Studio, Kerf Studio, Tapeline Studio, and the ICE design-thinking programme sites.

Served by a Cloudflare Worker (`design-sankhacooray`): static assets from `public/`, plus `src/worker.js`, whose
`/api/og?url=` route fetches a project site's current `og:image` (allowlisted hosts only, cached 1 h). Cards with an
`og` page in `projects.json` show a local copy of that share image, then fade to the live one.
The VS Code workspace for the hub and its projects is `../design-sankhacooray-com.code-workspace`.

## Files

| Path | Purpose |
|------|---------|
| `projects.json` | The hub's content: design areas, projects, roles, years, live links |
| `footer.html` | One-line footer, placed in the last section |
| `template.html` | Page template (CSS + JS inline): studio palette, artboard hero, design-glyph particles |
| `build.py` | Renders `public/index.html`; packs small areas side by side in balanced rows |
| `public/thumbs/` | Local copies of each project's share image (800×420); ICE 2025 is a screenshot |
| `public/og.png` | 1200×630 share image, rendered from `og/og.html` |
| `public/icon*.png`, `icon.svg`, `apple-touch-icon.png`, `manifest.webmanifest` | Icons + PWA manifest |
| `src/worker.js` | Serves assets; `/api/og` live share-image proxy |
| `wrangler.toml` | Worker config + custom domain (creates the DNS record) |

Roles on each card: **Mine** (I built and run it), **Co-owned** (shared ownership — the ICE programme sites),
**Contributed** (belongs to a colleague — ICEHub). Only live sites are linked; several source repositories are private.

## Edit, preview, deploy

```
python3 build.py
python3 -m http.server 4803 --bind 127.0.0.1 -d public
CLOUDFLARE_ACCOUNT_ID=9a1da48a0f61e7cebcba39517856d87e npx wrangler deploy
```

Share image: edit `og/og.html`, then render it and bump `?v=N` on `og:image`/`twitter:image` in `template.html`:

```
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --hide-scrollbars \
  --force-device-scale-factor=1 --window-size=1200,630 --virtual-time-budget=6000 \
  --allow-file-access-from-files --screenshot=public/og.png "file://$PWD/og/og.html"
```

After deploying, curl the page, `og.png` and the icons to confirm 200s and the right content types.
