# Changelog

## 1.1.0

- Remove live URL audit mode: codebase-only evidence; do not fetch or browse deployed sites or third-party page content (clears Snyk W011 third-party content exposure)
- Mark runtime-only checks (live headers, crawler HTML, social debugger previews) as **NOT VERIFIED**
- Update README and references for the narrowed scope

## 1.0.1

- Harden live URL audits against indirect prompt injection: treat remote HTML, discovery files, and headers as untrusted evidence only; stay on the user-supplied origin; prefer codebase evidence over contradictory live directives
- Soften live-audit wording (availability of evidence vs. imperative fetch-and-act) for agent-scan W011 triage; document residual live-mode exposure in README

## 1.0.0

Initial public release of `web-presence-audit`.

- Agent Skill for auditing public web presence across SEO, discovery (robots, sitemap, JSON-LD), social sharing (Open Graph / Twitter cards), security headers, accessibility (WCAG-oriented), and production readiness
- Supports codebase audits, live URL audits, and combined evidence with explicit **PASS** / **NOT VERIFIED** handling
- Progressive disclosure via `references/` for SEO, social sharing, discovery, security headers, accessibility, and framework notes
- Preserves practical inventory and remediation guidance from the prior private skill, generalized for open-source use
