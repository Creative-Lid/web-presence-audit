# Web Presence Audit

An Agent Skill that audits a website or web app for public-facing discoverability: SEO, social sharing cards, crawler discovery, security headers, and accessibility.

## What it audits

- **SEO** — titles, descriptions, canonicals, robots/indexability, headings, sitemaps, obvious technical issues
- **Discovery** — robots.txt, sitemap.xml, JSON-LD/Schema.org, optional AI discovery files (`llms.txt`, etc.)
- **Social sharing** — Open Graph, Twitter/X cards, share images and URLs
- **Security headers** — CSP, HSTS, nosniff, Referrer-Policy, Permissions-Policy, frame protection, and related public headers
- **Accessibility** — WCAG-oriented checks (semantics, labels, alt text, keyboard/focus, forms, landmarks) without claiming legal ADA certification
- **Production readiness** — icons/manifest when relevant, HTTPS/redirects, localhost/staging leaks in public metadata

## Why use it

Agents are good at checklists and bad at restraint. This skill pushes a practical engineering audit: inventory first, require evidence, avoid false positives, and separate **PASS** from **NOT VERIFIED**. It works against a **codebase**, a **live URL**, or both.

## Installation

Using the [skills](https://skills.sh) CLI:

```bash
npx skills add Creative-Lid/web-presence-audit
```

Compatible with Agent Skills-capable tools such as Cursor, Claude Code, Codex, GitHub Copilot, OpenCode, and others that load `SKILL.md` packages.

## Usage examples

Once installed, ask your agent naturally:

```text
Audit this website for SEO, social sharing, discoverability, security headers, and accessibility.
```

```text
Audit this codebase before production launch.
```

```text
Check whether this Next.js site is configured correctly for SEO and social sharing.
```

```text
Check robots.txt, sitemap.xml, JSON-LD, llms.txt, Open Graph, security headers, and WCAG issues.
```

```text
Audit https://example.com and prioritize anything I should fix before launch.
```

By default the skill **reports findings**. Ask explicitly if you want it to implement fixes in the repo.

## What the audit checks

| Area | Examples |
| --- | --- |
| SEO | Title/description/canonical, indexability, sitemap/robots alignment |
| Discovery | Structured data, crawler files, optional `llms.txt` (labeled as emerging) |
| Social | `og:*`, Twitter cards, absolute share image URLs |
| Security | Modern response headers; obsolete headers called out separately |
| Accessibility | Skip links, labels, focus, landmarks, forms; limitations stated clearly |
| General | Favicons, manifest when appropriate, host redirects, placeholder URL leaks |

Detailed guidance lives under [`references/`](references/).

## Codebase audits vs live URL audits

| Mode | What the agent uses | What you get |
| --- | --- | --- |
| **Codebase** | Source, configs, static assets, framework conventions | File-specific findings and fix guidance |
| **Live URL** | HTTP fetches / browser tools when available | Rendered metadata, headers, robots/sitemap verification |
| **Both** | Combined evidence | Strongest report; conflicts called out |

Live checks depend on network and tool access. If the environment cannot fetch the site, the skill must list those items under **Not Verified**—not invent passes.

## Supported agents

Any agent that supports the [Agent Skills](https://agentskills.io) format and can read this repository’s root `SKILL.md`. Optional tools (browser, curl, Lighthouse, axe) improve coverage but are not required for a useful source audit.

## Limitations

- Not a penetration test or vulnerability scan
- Not a performance lab report
- Not a legal ADA determination or WCAG certification
- Not a guarantee of search ranking outcomes
- Emerging files like `llms.txt` are optional conventions, not formal SEO requirements
- Client-rendered SPAs may limit what crawlers and this audit can verify without runtime tooling

## Repository structure

```text
/
├── SKILL.md                      # Installable skill (required)
├── README.md
├── LICENSE
├── CHANGELOG.md
├── .gitignore
└── references/
    ├── seo.md
    ├── social-sharing.md
    ├── discovery.md
    ├── security-headers.md
    ├── accessibility.md
    └── framework-notes.md
```

## Contributing

Issues and pull requests that improve accuracy, reduce false positives, or update framework discovery notes are welcome. Keep `SKILL.md` concise; put deep checklists in `references/`.

## License

MIT — see [LICENSE](LICENSE).
