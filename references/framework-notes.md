# Framework notes

Inspect the **installed** framework and version in the target repo (package.json, lockfile, config). Treat locations below as discovery hints, not guarantees—conventions change across majors.

## Detect the stack

Look for: `next`, `nuxt`, `astro`, `@remix-run/*`, `@sveltejs/kit`, `vite`, `react`, `vue`, `angular`, plain `index.html`, etc. Also note hosting signals: `vercel.json`, `netlify.toml`, `amplify.yml`, Cloudflare `_headers`, nginx configs.

## Next.js (App Router)

Common paths:

- `app/layout.tsx` — root metadata, fonts, `lang`
- `app/**/page.tsx` — per-route UI
- `generateMetadata` / `metadata` exports — titles, descriptions, Open Graph, Twitter, robots
- `metadataBase` — absolute URL prefix for canonical/OG
- `app/robots.ts`, `app/sitemap.ts`, `app/manifest.ts`
- `app/opengraph-image.*`, `icon.*`, `apple-icon.*`
- `next.config.*` — `headers`, redirects, images
- `middleware.ts` — auth gates, redirects, sometimes headers
- `public/` — static robots/llms/icons if not generated

Notes:

- Nested layouts inherit metadata; check overrides carefully.
- Client components cannot export `metadata`—misplacement causes missing head tags.
- Confirm whether the project is App Router (`app/`) vs Pages Router (`pages/`).

## Next.js (Pages Router)

- `pages/_document.tsx` — HTML shell, `lang`
- `pages/_app.tsx` — global head via `next/head` or third-party SEO plugins
- `pages/**` + `next/head` per page
- `public/robots.txt`, `public/sitemap.xml` or generated scripts
- `next.config.*` headers/redirects

## React + Vite (SPA)

- `index.html` — often the only static head; SPA metadata may be incomplete for crawlers
- React Helmet / `unhead` / router meta libraries if present
- `vite.config.*`, `public/` assets
- Flag crawler visibility limits honestly; recommend SSR/prerender when SEO is a goal

## Astro

- `src/layouts/*`, `src/pages/*`
- `astro.config.*` site URL / integrations (sitemap)
- `<head>` in layouts; per-page frontmatter
- `public/` for static discovery files
- View Transitions / islands: still prefer static head for share tags

## Nuxt

- `nuxt.config.*` — `app.head`, route rules, headers
- `app.vue`, `pages/*`, `composables/useSeoMeta` / `useHead`
- Server routes or modules for robots/sitemap
- Nitro headers / routeRules for security headers

## SvelteKit

- `src/routes/+layout.svelte`, `+page.svelte`
- `+page.ts` / `+layout.ts` for data; `<svelte:head>` for tags
- `static/` for robots, icons, llms
- `hooks.server.ts` for headers/redirects

## Remix / React Router frameworks

- `app/root.tsx` — meta exports, links, headers
- Route `meta` functions per page
- `public/` static files
- Check version (Remix vs RR7) before assuming APIs

## Vue (non-Nuxt)

- `index.html` + vue-router meta + `@unhead/vue` / vue-meta patterns
- Treat SEO similarly to other CSR apps unless SSG/SSR is configured

## Static HTML

- Each HTML file’s `<head>` and body landmarks
- Folder-level `robots.txt`, `sitemap.xml`, icons
- Server/CDN header config outside the repo may still apply—mark NOT VERIFIED if inaccessible

## Shared audit tips (all stacks)

1. Find the **canonical site URL** configuration first.
2. Trace metadata from root layout → nested layout → page.
3. Grep carefully for `noindex`, `robots`, `openGraph`, `twitter`, `application/ld+json`, `Content-Security-Policy`, `headers(`.
4. Confirm build/runtime: SSG, SSR, CSR, or hybrid—metadata behavior differs.
5. Prefer the host’s actual header mechanism when recommending security header fixes.
6. When unsure of API details, read the project’s installed docs/examples in `node_modules` or the framework version docs—do not invent deprecated APIs.
