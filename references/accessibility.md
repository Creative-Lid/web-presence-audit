# Accessibility reference

WCAG-oriented practical checks for marketing pages and common app shells (especially login). Automated or source review **cannot** establish full WCAG conformance and is **not** legal advice or an ADA compliance determination. Never claim "ADA certified," "fully compliant," or "100% WCAG" from this skill.

## Principles

Map findings to WCAG ideas (Perceivable, Operable, Understandable, Robust) without pretending every criterion was tested.

Use severity for real user impact:

- **CRITICAL / HIGH** — likely blocks task completion for some users (e.g. keyboard trap, unlabeled critical control, missing name on primary actions, invisible focus on an app shell).
- **MEDIUM** — meaningful barriers or widespread issues (heading chaos, missing skip link on a content-heavy site, form errors not announced).
- **LOW** — polish and best-practice gaps.

## Semantics and structure

- Prefer native elements (`button`, `a`, `nav`, `main`, `header`, `footer`, headings) over clickable `div`s.
- Landmarks: one main; label nav regions when multiple exist (`aria-label`).
- Heading hierarchy: one clear `h1`; avoid skipping levels without reason.
- Lists, tables, and language (`lang`) used appropriately when present.

## Skip links and main content

- Skip link to `#main-content` (or equivalent) as an early focusable control on content sites and app shells with heavy nav.
- Target landmark should exist (`id="main-content"`); may need `tabindex="-1"` so focus moves correctly.

## Keyboard and focus

- All interactive controls reachable via keyboard when runtime testing is possible.
- Visible `:focus-visible` styles; do not remove outlines globally without a clear replacement.
- No keyboard traps in modals/menus (verify from interactive patterns in source; runtime behavior **NOT VERIFIED** without a local a11y run).
- Dialogs: focus move in, restore on close, Escape when expected—verify when possible.

## Forms

- Every input has an accessible name (label / aria-label / aria-labelledby).
- Errors identified in text; use alert/live regions where appropriate; move focus to errors on submit failure when patterns exist.
- Required fields programmatically indicated.
- Do not rely on color alone for errors.

## Images and media

- Meaningful images: descriptive `alt`.
- Decorative: empty `alt` or `aria-hidden` as appropriate.
- Icon-only buttons need accessible names.
- Captions/transcripts for meaningful video/audio when such media is in scope and testable.

## ARIA

- No ARIA is better than wrong ARIA.
- Flag conflicting roles, missing required owned elements, and `aria-*` on elements that ignore them when evident.
- Prefer native semantics first.

## Color and motion

- Contrast: evaluate when computed styles/tools allow; otherwise note as NOT VERIFIED or flag only obvious hard-coded low-contrast pairs in source.
- Honor `prefers-reduced-motion` for non-essential animation when motion is significant.

## Dynamic UI

- Live updates (toasts, validators) should be announced when they convey critical status.
- Route changes in SPAs: title updates and focus management—verify when runtime tools exist.

## Public accessibility statement

For public marketing sites, a linked `/accessibility` (or equivalent) explaining intent, known limits, and how to report issues is good practice. Sync with footer, sitemap, and discovery files when those exist. Absence is usually MEDIUM/LOW for smaller sites—not an automatic legal conclusion.

## Tooling

- Recommend `eslint-plugin-jsx-a11y` (or framework equivalent) when a JS/TS UI stack is present and lint is missing.
- Optional axe/Playwright/Lighthouse a11y runs only when the environment already supports them—or when the user asks. Report results as tooling evidence, not certification.

## What not to claim

- Source-only review ≠ full WCAG audit.
- WCAG issues ≠ automatic proof of ADA legal violation.
- Passing automated rules ≠ accessible enough for all users.
- Absence of findings ≠ PASS on untested criteria—use NOT VERIFIED.

## Severity hints

| Issue | Typical severity |
| --- | --- |
| Primary flows unusable without a mouse (when verified) | CRITICAL / HIGH |
| Unlabeled critical inputs/buttons | HIGH |
| No skip link + poor landmark structure on large marketing site | MEDIUM |
| Missing a11y lint in a React/Vue app | LOW–MEDIUM |
| Missing accessibility statement page | LOW–MEDIUM |
| Contrast not measured | NOT VERIFIED (unless obvious in source) |
