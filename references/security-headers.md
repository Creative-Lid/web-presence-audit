# Security headers reference

Defensive review of **public HTTP response headers** for websites. This is **not** a penetration test, vulnerability assessment, or cookie-security certification. Flag obsolete headers separately; do not recommend them as modern requirements.

## Modern headers to evaluate

| Header | Purpose (short) |
| --- | --- |
| `Content-Security-Policy` | Mitigate XSS/content injection by controlling allowed sources |
| `Strict-Transport-Security` | Force HTTPS on subsequent visits (HTTPS only; careful with preload) |
| `X-Content-Type-Options` | `nosniff` — reduce MIME sniffing risks |
| `Referrer-Policy` | Limit referrer leakage |
| `Permissions-Policy` | Restrict powerful browser features |
| Frame protection | Prefer CSP `frame-ancestors`; legacy `X-Frame-Options` still seen |
| `Cross-Origin-Opener-Policy` | Isolate browsing context (when appropriate) |
| `Cross-Origin-Resource-Policy` | Control cross-origin resource reads (when appropriate) |

Also note (when visible): whether `Server` / `X-Powered-By` unnecessarily fingerprint the stack—usually LOW hardening, not a vulnerability by itself.

## Reasonable baselines (not one-size-fits-all)

Exact values depend on framework, CDN, and third parties. A cautious public-site baseline often includes:

- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin` (or stricter if compatible)
- `Permissions-Policy` disabling unused powerful features
- Frame denial via `Content-Security-Policy: frame-ancestors 'self'` (or `'none'`) and/or `X-Frame-Options: DENY` / `SAMEORIGIN`
- `Strict-Transport-Security` on **production HTTPS** responses (with thoughtful `max-age`; preload only when deliberately operated)

CSP should be designed around real script/style/image/connect needs. Prefer **Report-Only** while learning. Overly strict CSP that breaks the site is not a win. CSS-in-JS, inline scripts, analytics, and payment widgets commonly require deliberate entries—or CSP may be skipped until accounted for.

## Framework and hosting caveats

- Headers may be set in app config (`next.config` headers, middleware, Nitro/Nuxt route rules, platform `_headers`, CDN rules, reverse proxy). Audit the layer that actually ships to users.
- Preview/staging hosts may omit HSTS or use different CSP—do not confuse with production.
- Managed hosts sometimes inject headers; without a live response, treat config as best-effort and mark deployed header verification **NOT VERIFIED**.
- Long-cache on immutable static assets (icons, hashed JS/CSS, images) is a performance/caching concern adjacent to headers—mention when configuring platform headers, not as a CVE.

## HSTS cautions

- Only on HTTPS sites you control long-term.
- Too-short max-age is weak; huge max-age + preload is hard to undo—do not casually recommend preload.
- Avoid HSTS on hosts that must still speak plain HTTP for legacy reasons.

## CSP nuance

- Missing CSP ≠ automatic CRITICAL. Many sites rely on framework defaults + sanitization; treat missing CSP as hardening (often MEDIUM) unless other evidence shows active XSS exposure (out of scope to prove here).
- `unsafe-inline` / `unsafe-eval` weaken CSP—note them; they may be pragmatic.
- Do not demand nonce/hash setups unless the stack is ready.

## Frame ancestors

- Prefer CSP `frame-ancestors` over relying only on `X-Frame-Options`.
- Clickjacking risk depends on whether sensitive UI is frameable; public brochure sites may still want a policy.

## Permissions-Policy / Referrer-Policy / nosniff

- Missing `X-Content-Type-Options: nosniff` — common MEDIUM hardening gap.
- Missing Referrer-Policy or Permissions-Policy — usually MEDIUM/LOW depending on context.
- Do not invent long Permissions-Policy strings that break required features (camera, geolocation, payment) without checking product needs.

## Cookies (observational only)

If `Set-Cookie` is visible on responses in scope:

- Note absence of `Secure`, `HttpOnly`, or `SameSite` on session-like cookies as observational findings.
- Do not claim full session-security assurance from homepage headers alone.

## Obsolete / legacy headers — do not require

Flag if present as outdated noise; **do not** recommend adding:

- `X-XSS-Protection` (obsolete; can cause issues)
- `Expect-CT` (deprecated)
- `X-Frame-Options` alone as the *only* modern story is incomplete, but removing a working XFO without CSP replacement can be a regression—prefer adding `frame-ancestors`, then rationalize duplication.

## Severity hints

| Issue | Typical severity |
| --- | --- |
| Sensitive app framable with no frame protection | HIGH |
| Production site on HTTPS with no HSTS | MEDIUM (context-dependent) |
| No CSP on a complex script-heavy app | MEDIUM hardening |
| Missing nosniff / Referrer-Policy | MEDIUM / LOW |
| Verbose `X-Powered-By` | LOW |
| Missing CSP on a static brochure site | LOW / MEDIUM |

Always separate **hardening recommendations** from **demonstrated vulnerabilities**. Never instruct bypassing auth, exploiting XSS, or destructive testing.
