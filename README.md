# Web Presence Audit

An Agent Skill that audits a website or web app **codebase** for public-facing discoverability: SEO, social sharing cards, crawler discovery, security headers, and accessibility.

## What it audits

- **SEO** — titles, descriptions, canonicals, robots/indexability, headings, sitemaps, obvious technical issues
- **Discovery** — robots.txt, sitemap.xml, JSON-LD/Schema.org, optional AI discovery files (`llms.txt`, etc.)
- **Social sharing** — Open Graph, Twitter/X cards, share images and URLs
- **Security headers** — CSP, HSTS, nosniff, Referrer-Policy, Permissions-Policy, frame protection, and related config
- **Accessibility** — WCAG-oriented checks (semantics, labels, alt text, keyboard/focus, forms, landmarks) without claiming legal ADA certification
- **Production readiness** — icons/manifest when relevant, HTTPS/redirect config, localhost/staging leaks in public metadata

## Why use it

Agents are good at checklists and bad at restraint. This skill pushes a practical engineering audit: inventory first, require evidence, avoid false positives, and separate **PASS** from **NOT VERIFIED**. It uses repository source, configuration, and static assets; deployed-only behavior stays **NOT VERIFIED**.

## Installation

Using the [skills](https://skills.sh) CLI:

```bash
npx skills add Creative-Lid/web-presence-audit
```

Compatible with Agent Skills-capable tools such as Cursor, Claude Code, Codex, GitHub Copilot, OpenCode, and others that load `SKILL.md` packages.

## Usage examples

Once installed, ask your agent naturally:

```text
Audit this codebase for SEO, social sharing, discoverability, security headers, and accessibility.
```

```text
Audit this codebase before production launch.
```

```text
Check whether this Next.js site is configured correctly for SEO and social sharing.
```

```text
Check robots.txt, sitemap.xml, JSON-LD, llms.txt, Open Graph, security headers, and WCAG issues in this repo.
```

By default the skill **reports findings**. Ask explicitly if you want it to implement fixes in the repo.

## What the audit checks

| Area | Examples |
| --- | --- |
| SEO | Title/description/canonical, indexability, sitemap/robots alignment |
| Discovery | Structured data, crawler files, optional `llms.txt` (labeled as emerging) |
| Social | `og:*`, Twitter cards, absolute share image URLs |
| Security | Header/middleware/CDN config; obsolete headers called out separately |
| Accessibility | Skip links, labels, focus, landmarks, forms; limitations stated clearly |
| General | Favicons, manifest when appropriate, redirect config, placeholder URL leaks |

Detailed guidance lives under [`references/`](references/).

## Evidence model

| Source | What the agent uses | What you get |
| --- | --- | --- |
| **Codebase** | Source, configs, static assets, framework conventions | File-specific findings and fix guidance |
| **Deployed behavior** | Outside repository evidence | List under **Not Verified** |

## Supported agents

Any agent that supports the [Agent Skills](https://agentskills.io) format and can read this repository’s root `SKILL.md`.

## Limitations

- Not a penetration test or vulnerability scan
- Not a performance lab report
- Not a legal ADA determination or WCAG certification
- Not a guarantee of search ranking outcomes
- Emerging files like `llms.txt` are optional conventions, not formal SEO requirements
- Client-rendered SPAs may limit what can be verified from source alone
- Deployed headers, crawler output, and social-platform previews are **NOT VERIFIED** unless repository evidence establishes the relevant behavior.

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
