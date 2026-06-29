# Shorelinev2 — Static Preview Build

Static export of the Phase 2 Shoreline Sightseeing site, deployed via Vercel.

## What this is

A static HTML mirror of the local WordPress development install at the time of export. Every page from `/`, `/tours/*`, `/water-taxis/*`, `/blog/*`, plus all assets (CSS/JS/images/video/fonts) is included as flat files.

## What works

- All static page renders (HTML, CSS, JS, images, fonts, video)
- Internal navigation between pages
- Inline JavaScript including Leaflet tour maps with animated boat routes
- Stackable blocks rendered at export time
- Yoast SEO metadata baked in

## What doesn't work (server-side dependencies)

- WordPress search (`?s=` query) — would need a JS-only replacement
- Ventrata booking widget — requires its own JS bundle from Ventrata's CDN (already linked, loads fine at runtime)
- WordPress comments / forms — purely static
- WP admin / login

## Vercel setup

`vercel.json` configures:
- `cleanUrls: true` — strips `.html` extensions
- `trailingSlash: true` — appends `/` to paths (matches WP URL style)
- Long cache headers on static assets

Vercel auto-detects this as a static site and deploys directly from the repo root.

## Re-exporting

To regenerate this build from the local WP install:

```bash
wget --mirror --page-requisites --convert-links --adjust-extension \
  --no-host-directories --no-check-certificate \
  --restrict-file-names=windows \
  --reject 'wp-admin*,wp-login*,xmlrpc*,*?author=*,*?p=*,*?attachment_id=*,*feed*,*trackback*,*comments*,*\?replytocom*' \
  --exclude-directories=wp-admin,wp-json \
  https://site-shorelinesigh1-live-1779830508-d3enkipvrvpwow.local/
```

Then run the Python rewrite step to replace the local hostname with empty string (making URLs root-relative).
