# Social sharing reference

Open Graph and Twitter/X card checks for public pages. Skip or mark N/A for private/internal applications unless the user explicitly wants share previews.

## Core tags

### Open Graph

Typical minimum for a public marketing page:

- `og:title`
- `og:description`
- `og:url` (canonical public URL)
- `og:type` (often `website`; `article` etc. when appropriate)
- `og:image` (absolute URL)
- Optional but useful: `og:site_name`, `og:locale`

### Twitter / X

- `twitter:card` — usually `summary_large_image` when a large share image exists
- `twitter:title`, `twitter:description`, `twitter:image`
- `twitter:site` / creator handles only when a real brand account exists—do not invent handles

Many frameworks map OG + Twitter from one shared metadata helper. Prefer DRY helpers over duplicated per-page strings.

## Share image

- Target roughly **1200×630** for large cards (common convention; platforms vary).
- Image URL must be **absolute** `https://…` on production.
- Verify the asset exists (static file, generated route, or CDN) when tools allow.
- Avoid requiring a separate LinkedIn-only image unless there is a demonstrated need.
- Placeholder, broken, or localhost image URLs on production are HIGH.

## Common implementation mistakes

- Relative `/og.png` without a site URL / `metadataBase` → broken previews.
- Title/description mismatch between document meta and OG/Twitter.
- One global share image and title for every route with no per-page overrides on important landings.
- Client-only injection of OG tags (many scrapers do not execute JS reliably)—prefer SSR/SSG/server metadata APIs.
- Stale platform caches after deploy—mention debuggers as a smoke step, not as a code defect.
- Shipping `twitter:card=summary_large_image` without a usable large image.

## Validation considerations

This skill audits source only (no live fetches):

- Confirm helpers emit absolute URLs given the configured site origin.
- Flag likely relative URL bugs and missing default images as code-level findings.
- Mark live preview rendering and platform debugger results **NOT VERIFIED**.
- Optionally mention platform debuggers (Facebook Sharing Debugger, X Card Validator, LinkedIn Post Inspector) as **manual** smoke steps for the user—not as agent network tasks.

## Severity hints

| Issue | Typical severity |
| --- | --- |
| No OG/Twitter tags on a public marketing site meant to be shared | HIGH |
| OG image localhost / 404 | HIGH |
| Missing per-page titles on major campaigns | MEDIUM |
| Missing `og:site_name` or locale | LOW |
| OG on authenticated-only app | N/A unless requested |
