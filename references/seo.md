# SEO reference

Practical technical SEO checks for **codebase** audits. Verify with source evidence; skip items that do not apply. Mark runtime-only checks **NOT VERIFIED**.

## Crawlability and indexability

- Public pages should be reachable without auth walls for content meant to rank.
- Check `robots` meta / header, `X-Robots-Tag`, and `robots.txt` Allow/Disallow.
- Private app surfaces (`/login`, `/account`, `/admin`, `/dashboard`, `/settings`, APIs) usually belong in `noindex` and matching robots disallow—not "broken SEO."
- Staging/preview hosts should not be indexable in production search results (password, header auth, or robots/`noindex`).
- Client-only apps: confirm whether critical metadata and content exist in initial HTML or only after JS. If crawlers see an empty shell, flag with evidence—not speculation.

## Titles and descriptions

- Every indexable HTML document needs a unique, descriptive `<title>` (or framework title API).
- Meta description should be unique where practical; missing is MEDIUM for marketing pages, often N/A for app chrome.
- Avoid placeholder copy ("TODO", "Home", product-name-only on every page).
- Prefer a title template or shared brand strategy over one-off inconsistent patterns.
- `lang` on the root document should match the primary content language.

## Canonicalization and duplicates

- Indexable pages should declare a canonical URL when duplicates, tracking params, or multiple hosts are possible.
- Align canonical, `metadataBase` / site URL env, Open Graph `og:url`, and sitemap entries on one public origin.
- Apex ↔ `www` (or alternate hosts): prefer one permanent redirect target; conflicting hosts are HIGH when both resolve publicly.
- Trailing-slash and `http`→`https` consistency matter for duplicate URL risk.

## Headings and content structure

- One clear `h1` per page when auditing marketing/content templates.
- Heading levels should not skip arbitrarily in a way that breaks outline comprehension.
- Internal links: note obvious orphan templates or nav/footer mismatches when detectable; do not invent a full link-graph audit from a few files.

## Images (SEO + accessibility overlap)

- Meaningful images need descriptive `alt`; decorative images empty `alt` or appropriately hidden.
- Prefer real filenames/CDN URLs over `blob:` or missing assets in public metadata.
- OG/share images are covered in [social-sharing.md](social-sharing.md); still flag broken image paths that affect SEO snippets when seen.

## Sitemap

- Presence of `sitemap.xml` (or framework `sitemap.ts` / generated equivalent) for multi-page public sites.
- Sitemap should list **public indexable** URLs only—exclude private app routes.
- URLs must use the canonical https origin (no localhost, staging hosts, or http if production is https).
- Keep sitemap in sync with real routes, footer legal links, and robots `Sitemap:` directive.
- Single-page brochure sites: a minimal sitemap is optional polish (LOW / N/A), not CRITICAL.

## robots.txt

- Should exist for public production sites; must not accidentally `Disallow: /` on production marketing content.
- Include `Sitemap:` absolute URL when a sitemap exists.
- Optional `Host` is non-standard; do not require it.
- Never treat robots.txt as authentication or a security control.
- AI crawler rules (GPTBot, etc.) are product/policy choices—document them; do not equate them with `llms.txt`.

## Redirects and URL hygiene

- Broken redirects, redirect chains, and mixed canonical targets are actionable when observed.
- Public metadata must not reference `localhost`, internal hostnames, or placeholder domains in production builds.
- Check env examples and deploy docs for the intended public site URL.

## Structured data (SEO relationship)

- JSON-LD can enhance understanding but is not a substitute for crawlable content and sound titles/canonicals.
- Only recommend types that match the page (Organization, WebSite, SoftwareApplication, Article, Product, FAQPage, etc.).
- Invalid or misleading schema is worse than none—validate shape when recommending changes.
- Details: [discovery.md](discovery.md).

## Common technical issues to watch

- Missing `metadataBase` / absolute URL base causing relative canonical or OG URLs.
- Duplicate conflicting meta tags from layout + page + hard-coded `<head>`.
- Marketing pages accidentally inheriting app-wide `noindex`.
- Sitemap or robots generated at build time with wrong env URL.
- Soft 404s (200 responses for missing content) when detectable.

## Severity hints

| Issue | Typical severity |
| --- | --- |
| Production public site globally `noindex` or `Disallow: /` | CRITICAL |
| Canonical/OG/sitemap pointing at localhost or staging on prod | CRITICAL / HIGH |
| Missing unique titles on key landing pages | HIGH |
| Missing meta descriptions on key landing pages | MEDIUM |
| Missing sitemap on a large public site | MEDIUM |
| Missing sitemap on a one-page site | LOW / N/A |
| Keywords meta stuffed or outdated | LOW / ignore |
