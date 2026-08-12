# Discovery reference

Crawl and machine-discovery surfaces: robots, sitemaps, canonical signals, structured data, and AI-oriented files.

## Established standards

Treat these as mainstream when a site is public:

| Mechanism | Role |
| --- | --- |
| `robots.txt` | Crawler hints; not security |
| Sitemap XML | URL inventory for crawlers |
| Canonical URLs | Duplicate consolidation |
| HTML meta robots / `X-Robots-Tag` | Per-page index/follow control |
| JSON-LD / Schema.org | Structured meaning for pages/entities |

## robots.txt

Check:

- File reachable at `/robots.txt` on the public origin.
- Does not block public marketing content unintentionally.
- Disallows private app/API prefixes when those exist.
- `Sitemap:` points to an absolute https sitemap URL.
- Staging rules are not copied blindly into production.

## sitemap.xml

Check:

- Discoverable via robots and/or conventional `/sitemap.xml` (or sitemap index).
- Contains public indexable URLs only.
- Uses canonical host and https.
- Stays aligned with real routes and footer/legal links.
- Lastmod/changefreq/priority: optional; wrong priorities are LOW noise—do not over-emphasize.

Framework generators (`app/sitemap.ts`, `nuxt-simple-sitemap`, Astro integrations, etc.) should be inspected for env-dependent origin bugs.

## Canonical URLs

- Prefer one canonical per indexable page.
- Align with redirects and sitemap host.
- Paginated/filtered/faceted URLs: canonical strategy should be intentional when present.

## JSON-LD / Schema.org

Inject as `application/ld+json` when used.

Recommend only fitting types:

- **Organization** — name, url, logo, contact points; `sameAs` only for real profiles
- **WebSite** — with publisher when appropriate; `SearchAction` only if a public site search exists
- **SoftwareApplication** / product equivalents — when shipping a product
- **Article** / **BlogPosting** — article templates
- **Product** / **Offer** — commerce
- **FAQPage** — only when visible FAQ content exists on the page

Flag:

- Empty or placeholder values
- Schema that contradicts visible content
- Multiple conflicting Organization blocks without reason

Structured data is **not** required on every page. Missing FAQ schema on a non-FAQ page is not a finding.

## AI / LLM discovery (emerging)

These are **optional conventions**, not formal SEO requirements. Never score a site as failing SEO solely for omitting them. Label clearly as emerging in reports.

### `llms.txt`

- Optional markdown at `/llms.txt` summarizing the product and linking key public pages.
- Useful for public products that want to guide LLM crawlers; still not universal.
- If present, keep links accurate and include accessibility/contact pages when they exist.

### `llms-full.txt`

- Optional longer companion; even less standardized than `llms.txt`.
- Do not require it.

### `ai.txt`

- Occasional machine-oriented discovery file pointing at sitemap/robots/llms/security.
- Nonstandard; optional.

### Crawler directives

- Robots rules for AI user-agents are policy choices.
- Do not claim that GPTBot rules replace good public documentation or `llms.txt`.
- Do not claim experimental files are required by Google/Bing.

### Content accessibility for AI/search crawlers

- Important public content should be available without login when discoverability is a goal.
- Blocked resources, empty CSR shells, or `noindex` on marketing pages undermine both traditional and AI discovery—verify before asserting.

## Trust / contact discovery (related)

When auditing public production sites, optionally note:

- `/.well-known/security.txt` — Contact, Expires, Preferred-Languages, Canonical (good practice, not SEO)
- `humans.txt` — optional novelty; LOW / N/A

## Usually skip unless needed

ads.txt, app-ads.txt, RSS/Atom, hreflang, OpenSearch, Apple/Google app association files (only if native apps/Universal Links are in scope—never invent team IDs).

## Severity hints

| Issue | Typical severity |
| --- | --- |
| Production `Disallow: /` or global noindex on public site | CRITICAL |
| Sitemap/robots pointing at wrong host | HIGH |
| Invalid/misleading JSON-LD on key pages | MEDIUM–HIGH |
| Missing robots/sitemap on large public site | MEDIUM |
| Missing `llms.txt` | LOW / N/A (emerging) |
| Missing `security.txt` | LOW (trust), not SEO |
