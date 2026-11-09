# Changelog

## 1.2.0

- Define the audit entirely in terms of repository source, configuration, and static assets
- Classify deployed-only behavior as **NOT VERIFIED**
- Remove obsolete workflow descriptions from historical release notes so packaged documentation matches the current scope

## 1.1.0

- Restrict evidence to the codebase
- Mark deployment-only checks as **NOT VERIFIED**
- Update README and references for the narrowed scope

## 1.0.0

Initial public release of `web-presence-audit`.

- Agent Skill for auditing public web presence across SEO, discovery (robots, sitemap, JSON-LD), social sharing (Open Graph / Twitter cards), security headers, accessibility (WCAG-oriented), and production readiness
- Supports repository-based audits with explicit **PASS** / **NOT VERIFIED** handling
- Progressive disclosure via `references/` for SEO, social sharing, discovery, security headers, accessibility, and framework notes
- Preserves practical inventory and remediation guidance from the prior private skill, generalized for open-source use
