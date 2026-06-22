# Roadmap — EconomicValueOfSAHS

Derived from `docs/architecture/COMPLIANCE_REPORT.md` (Tier 1 compliance audit, 2026-05-23). This is a single-page break-even calculator (`index.html`, `styles.css`, `script.js`) deployed to Cloudflare Pages. The audit found 56% of applicable Tier 1 principles met or partially met; the items below are the prioritized remediation work to reach launch-ready.

## Status notes (not counted toward the bar)

- Compliance audit: 14 Met, 8 Partial, 17 Not Met, 2 N/A (22 of 39 applicable principles met or partial = 56%).
- Tier 1 — Static/Marketing (0/10 escalation questions yes).
- Next recommended compliance review: 2026-08-23 (90 days).

## Critical (Security / Compliance Risk)

- [ ] Add a `_headers` file at repo root for Cloudflare Pages (CSP allow-listing `'self'` + pinned jsDelivr URL, plus HSTS, X-Content-Type-Options: nosniff, Referrer-Policy: strict-origin-when-cross-origin, X-Frame-Options: DENY).
- [ ] Pin Chart.js to a specific version and add Subresource Integrity (or vendor Chart.js into the repo to remove the third-party runtime dependency entirely).
- [ ] Add the SEO baseline in `index.html`: `<meta name="description">`, canonical `<link>`, Open Graph + Twitter Card tags, `robots.txt`, and a one-URL `sitemap.xml` (registry marks `seo: required`).

## Important (Quality / Reliability Risk)

- [ ] Make the layout responsive: replace fixed `.container { width: 400px }` with a fluid width, remove the `height: 100vh` flex centring, and add a breakpoint at ~640px.
- [ ] Render a readable text result (not just charts) in a `<div role="status" aria-live="polite">` showing the computed break-even income and taxes.
- [ ] Label the canvases with `aria-label` (bar chart of cost breakdown; line chart of break-even income vs. tax rate).
- [ ] Replace `alert()` with inline field validation, including range checks (no negative costs; tax rate 0–100).
- [ ] Add a visible `:focus-visible` style on the Calculate button and inputs.
- [ ] Honour `prefers-reduced-motion` by disabling Chart.js animation when the media query matches.
- [ ] Add a minimal privacy notice stating no data is collected, no cookies are set, and that Chart.js is loaded from jsDelivr.

## Recommended (Best Practice)

- [ ] Establish a CSS token layer (`--color-brand`, `--color-text`, `--space-1`..`--space-4`) and reuse it.
- [ ] Strip `console.log` debug calls from `script.js`.
- [ ] Add a `package.json` with a minifier devDependency and an `npm audit` script in CI.
- [ ] Generate the remaining Tier 1 documentation stack (CLAUDE.md, design-tokens.md, site-spec.md, system-architecture.md, tier ADR).
- [ ] Tighten commit hygiene (Conventional Commits or at minimum descriptive messages).
