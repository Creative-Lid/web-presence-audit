---
name: web-presence-audit
description: >-
  Audits a website or web app for public web presence and technical
  discoverability: SEO metadata, Open Graph/Twitter cards, robots.txt,
  sitemap, JSON-LD/Schema.org, llms.txt and AI discovery, security headers
  (CSP, HSTS), icons/manifest, HTTPS/redirects, and WCAG/ADA-oriented
  accessibility. Use for pre-launch site health checks, SEO audits, social
  sharing metadata, structured data, crawler discoverability, security header
  reviews, accessibility/WCAG checks, or a full marketing web presence audit
  of a codebase or live URL. Inventory first; prefer accurate findings over
  speculative checklists; implement fixes only when asked.
license: MIT
metadata:
  author: Creative-Lid
  version: "1.0.0"
---

# Web Presence Audit

Practical engineering audit of a site's **public-facing** discoverability, sharing, security headers, and accessibility. Work from evidence in source code and/or a live URL. Prefer fewer accurate findings over speculative checklists.

## When to use

Activate when the user wants a web presence / pre-launch / SEO / social / discovery / security-headers / accessibility audit of a codebase or public URL.

Do **not** treat this as a penetration test, performance lab, legal ADA opinion, or WCAG certification.

## Scope first

1. Confirm target: **codebase**, **live URL**, or **both**.
2. Confirm depth: full audit (default) vs one category (SEO only, headers only, etc.).
3. Confirm mode: **report only** (default) vs **implement fixes** (only if the user asks).
4. Identify surface: marketing site, docs, app shell, or mixed. Private routes (`/login`, `/account`, `/admin`, `/dashboard`, …) are usually expected to be `noindex` / disallowed—not "missing SEO."

If the repo is a monorepo, locate the package that ships public HTML (`apps/web`, `apps/marketing`, `site/`, `web/`, or repo root). Prefer that package; do not invent paths from other products.

## Workflow

### 1. Detect context

- **Framework / runtime**: inspect lockfiles, configs, and entrypoints (see [references/framework-notes.md](references/framework-notes.md)).
- **Public origin**: site URL env, `metadataBase`, canonical host, deploy docs—pick one canonical origin when recommending fixes.
- **Capabilities**: note whether you can read source, fetch URLs, inspect response headers, or run a browser. Missing capability → **NOT VERIFIED**, never fabricate PASS.

### 2. Inventory before judging

Map what exists: document/head metadata, layouts, per-route overrides, `public/` / static assets, robots, sitemap, manifests, discovery files, header/middleware/CDN config, JSON-LD, icons, a11y patterns (skip link, landmarks, forms).

Mark items **present**, **missing**, or **N/A** (one-line reason). N/A examples: no native apps → skip AASA/asset links; no forms → skip form a11y depth; internal tool → OG/llms may be N/A; ordinary brochure site → PWA/manifest often N/A.

### 3. Audit by category

Load references on demand (do not dump all into context unless needed):

| Category | Reference |
| --- | --- |
| SEO | [references/seo.md](references/seo.md) |
| Discovery (robots, sitemap, JSON-LD, AI files) | [references/discovery.md](references/discovery.md) |
| Social sharing | [references/social-sharing.md](references/social-sharing.md) |
| Security headers | [references/security-headers.md](references/security-headers.md) |
| Accessibility | [references/accessibility.md](references/accessibility.md) |
| Framework locations | [references/framework-notes.md](references/framework-notes.md) |

For each finding: severity, category, evidence (file path, URL, header, or snippet), why it matters, concrete fix. No evidence → do not file as PASS or CRITICAL.

### 4. Codebase vs live site

**Codebase audit**

- Follow framework conventions; check shared defaults **and** page overrides.
- Reason about rendered output (SSR/SSG vs client-only metadata).
- Recommend exact files/paths. Edit code only when the user asks to fix or implement.

**Live URL audit**

- Fetch representative pages (home + 1–2 key templates), `/robots.txt`, sitemap (from robots or `/sitemap.xml`), discovery files, icons/manifest when linked.
- Inspect response headers on at least the document response.
- Distinguish verified observations from what tools could not check.
- If network/browser tools are unavailable: say so, audit source only if present, list live checks under **Not Verified**.

### 5. Report

Use the [Output format](#output-format) below. Then offer an ordered fix plan. Implement only on request; match repo conventions and package manager; prefer shared helpers over one-off page metadata.

## Severity model

| Level | Meaning |
| --- | --- |
| **CRITICAL** | Likely blocks indexing of public content, serious security exposure from public headers/config, major accessibility barrier, or severe public-facing breakage (e.g. production URLs pointing at localhost). |
| **HIGH** | Important SEO, sharing, discovery, security-header, or accessibility issues to fix before or soon after launch. |
| **MEDIUM** | Meaningful quality/consistency gaps; not immediate blockers. |
| **LOW** | Polish, optional enhancements, minor inconsistencies. |
| **PASS** | Explicitly verified with evidence. |
| **NOT VERIFIED** | Could not meaningfully test with available tools/environment. |

Do not mark PASS merely because a text search found no matches.

## False-positive avoidance (hard rules)

- Do not require a web manifest / PWA files for every site.
- Do not treat `llms.txt` / `ai.txt` as formal SEO requirements; label them as emerging conventions.
- Do not call every missing security header a vulnerability; consider host/CDN and architecture. Never recommend obsolete headers as modern requirements.
- Do not require Open Graph on private/internal apps.
- Do not assume every page should be indexed.
- Do not demand Schema.org types that do not fit the page.
- Do not treat `robots.txt` as a security boundary.
- Do not equate WCAG findings with legal ADA violations or claim compliance/certification from this audit.
- Do not assume client-rendered metadata works without checking framework/runtime behavior.
- Do not invent performance findings without measurement tooling/data.
- Do not invent a second backend or identity provider "for SEO."
- Keep secrets out of git; verification tokens belong in env / `.env.example` placeholders only.
- Sync public URL lists across footer/legal nav, robots, sitemap, and discovery files when those files exist or are recommended.

## Scoring

Use descriptive ratings by default: **Excellent** / **Good** / **Needs Work** / **Poor** / **Not Verified**.

Only use numeric scores if you apply an explicit, stated rubric for this run (what each band means and which verified checks drove the rating). Do not invent precise scores from vibes.

## Output format

```markdown
# Web Presence Audit

## Executive Summary
[Overall health and the most important issues]

## Scorecard
- SEO: …
- Discovery: …
- Social Sharing: …
- Security Headers: …
- Accessibility: …
- Overall: …

## Critical / High Priority Findings
For each:
- Severity:
- Category:
- Finding:
- Evidence:
- Why it matters:
- Recommended fix:
- Where: [file/path/URL]

## Category Findings
### SEO
### Discovery
### Social Sharing
### Security
### Accessibility
### General Web Presence

## Passed Checks
[Only explicitly verified]

## Not Verified / Limitations
[What could not be tested and why]

## Recommended Fix Order
[Concise ordered remediation; framework-specific snippets when helpful]
```

When the user asked for a single category, keep the same honesty rules but narrow the report.

## Fix mode (only when asked)

Suggested order when implementing gaps:

1. Site URL / canonical origin helper + root metadata defaults.
2. Share image (≈1200×630) + icons as needed + social metadata helpers.
3. Per-page titles/descriptions/canonicals; `noindex` private routes.
4. robots + sitemap + JSON-LD appropriate to the product.
5. Security headers + canonical-host redirect when the host allows (CSP carefully; Report-Only if unsure).
6. Discovery files (`llms.txt`, optional `ai.txt`, `.well-known/security.txt`) when the site is public and the user wants them—not as fake SEO blockers.
7. Accessibility: skip link, `#main-content`, focus-visible, labels/alt, heading order, form error announcements, reduced motion; public accessibility statement when appropriate; wire a11y lint if the stack supports it.
8. Search Console / Bing verification via env hooks if requested.
9. Smoke checklist in the summary (home, inner page, robots, sitemap, OG image, headers, skip link / focus).

Keep lists in sync. Summarize present / added / N/A. Do not commit unless asked.

## Missing tools

State limitations plainly. Partial audits are fine. Never claim live header, crawler, or browser checks that did not run.
