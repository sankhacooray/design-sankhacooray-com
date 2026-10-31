# design.sankhacooray.com

See README.md for build, preview (port 4803) and deploy (`python3 build.py` then `npx wrangler deploy`).

## Lighthouse
<!-- lighthouse-optimized: 2026-10-31 -->
- Baseline mobile: performance 67, accessibility 100, best-practices 100, SEO 100 (desktop perf 100, a11y 100).
- Fixed: non-blocking Google Fonts (print/onload + noscript); brand link aria-label removed so its name matches the visible text; card thumbs served as WebP with 400w/800w `srcset` (`build.py` picks `thumbs/<name>.webp` + `-400.webp` when present; JPGs kept for `og/og.html`); live `/api/og` share images now start after `load` + idle with `loading=lazy`; `public/_headers` gives thumbs/icons/og.png a 30-day TTL.
- Left: `/api/og` returns the projects' full 1200×630 PNGs with a 1 h TTL (by design: always the latest card). Resizing them needs Cloudflare Image Transformations on the zone. Shared scripts / gtag / CF beacon are handled centrally.
