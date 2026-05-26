# Compliance Report — EconomicValueOfSAHS

**Date:** 2026-05-23
**Tier:** 1 — Static/Marketing
**Framework Version:** Universal Web Development Principles v4.03.01
**Auditor:** /compliance (automated)

## Project Snapshot

A single-page break-even calculator (childcare vs. work income, with tax-rate inputs) that renders two Chart.js visualisations. Three files total: `index.html`, `script.js`, `styles.css`. No build system, no package manager, no tests, no CI. Chart.js is loaded from a third-party CDN (`cdn.jsdelivr.net`). Deploys to Cloudflare Pages. Git remote: `GetDigitalOS/EconomicValueOfSAHS`. No PII collected — all calculations happen client-side in memory.

## Summary

| Status | Count |
|--------|-------|
| ✅ Met | 14 |
| ⚠️ Partial | 8 |
| ❌ Not Met | 17 |
| ⬜ Not Applicable | 2 |

**Overall Compliance:** 56% of applicable Tier 1 principles met or partially met (22 of 39).

## Critical Gaps (Fix Before Launch)

1. **No Security Headers** — No `_headers` file for Cloudflare Pages, no CSP, HSTS, X-Frame-Options, X-Content-Type-Options, or Referrer-Policy. The page also loads Chart.js from `cdn.jsdelivr.net` without Subresource Integrity, so any CDN compromise executes arbitrary script in the user's browser.
2. **Third-party script with no SRI** (`index.html:9`) — `<script src="https://cdn.jsdelivr.net/npm/chart.js">` has no `integrity` or `crossorigin` attribute and pins no version. A pinned version + SRI hash is the minimum bar; a vendored copy is better.
3. **No SEO metadata** — No `<meta name="description">`, no Open Graph / Twitter tags, no canonical URL, no `robots.txt`, no `sitemap.xml`, no JSON-LD. The registry marks `seo: "required"` for this project but nothing is in place.
4. **No privacy disclosure** — The project does not collect data, but it does load a third-party CDN that observes the user's IP, user-agent, and referrer. Tier 1 principles require a privacy/cookie disclosure even when only analytics or third-party assets are involved.
5. **Tier mismatch in Project Hub registry** — Now corrected during this audit. Registry tier was `"unclassified"` while `PROJECT_CLASSIFICATION.md` resolves to Tier 1. Updated to `"1"` and set `last_audited` to today.

## Detailed Results

### Tier 1: Security Fundamentals

- ✅ **HTTPS Everywhere** — Cloudflare Pages serves over HTTPS by default with auto-managed certs; the only external resource (`cdn.jsdelivr.net`) is also HTTPS. No mixed content.
- ❌ **Security Headers** — No `_headers`, `_redirects`, `wrangler.toml`, or any other deployment config in the repo. No CSP, HSTS, X-Frame-Options, X-Content-Type-Options, or Referrer-Policy. Cloudflare Pages does not set these by default.
- ⚠️ **Input Validation** — Client-side `isNaN()` check exists (`script.js:11-20`), which is adequate for a no-backend calculator. There is no upper-bound or negativity check (e.g. a tax rate of `200` or `-50` silently produces nonsense output), and `parseFloat` happily accepts `"1e308"`. No server side exists, so server-side validation is N/A.
- ❌ **Dependency Hygiene** — No `package.json` and no `npm audit` possible. Chart.js is pulled live from `cdn.jsdelivr.net/npm/chart.js` with no version pin (always-latest) and no Subresource Integrity. Any future Chart.js release — or any CDN compromise — runs in the user's browser unreviewed.

### Tier 1: Design System Basics

- ❌ **Semantic Tokens** — Colours and spacing are inline hex/px literals throughout `styles.css` (`#007BFF`, `#0056b3`, `#f9f9f9`, `20px`, `8px`). No CSS custom properties, no token layer.
- ❌ **Token Hierarchy** — None. No primitives → semantic → component layering.
- ⚠️ **Consistent Spacing Scale** — Values used (`5px`, `8px`, `10px`, `20px`) loosely follow a 5px-based scale but it isn't codified. No documented scale.
- ❌ **Typography Scale** — Only `font-family: Arial, sans-serif` is set; everything else uses browser defaults. No defined scale, no fluid typography (`clamp()`).
- ⬜ **Icon System** — No icons used. N/A.
- ❌ **Responsive Breakpoint Strategy** — Fixed `.container { width: 400px }` (`styles.css:18`) with no `@media` queries. On viewports narrower than 400px the layout overflows; on larger screens it floats centred. No breakpoint strategy.

### Tier 1: Performance

- ⚠️ **Core Web Vitals Targets** — Not measured. Page is tiny so LCP and CLS are likely fine on cold load, but Chart.js is render-blocking (`script.js` is `defer` but the Chart.js CDN script is not deferred — `index.html:9` vs `:10`). No Lighthouse run on record.
- ❌ **Asset Optimization** — No images to optimise (so N/A by content), but CSS and JS are not minified. Chart.js comes minified from the CDN.
- ⬜ **Lazy Loading** — No images and no offscreen content. N/A.
- ✅ **CDN Delivery** — Cloudflare Pages serves static assets from the edge; Chart.js is also CDN-delivered.

### Tier 1: Accessibility (WCAG 2.1 AA)

- ⚠️ **Semantic HTML** — Has a single `<h1>`, real `<label for>` / `<input id>` pairs, and a real `<button>` (good). But there are no landmarks (`<main>`, `<header>`, `<footer>`), `<canvas>` elements are unlabeled, and clicking "Calculate" produces no text result — only visual charts that screen readers cannot interpret.
- ✅ **Keyboard Navigation** — All controls are native form elements; Tab/Enter work out of the box.
- ⚠️ **Color Contrast** — Body text on white (`#333` on `#ffffff`) passes. Button text white on `#007BFF` is ~4.5:1 (right at the edge). The bar-chart colour scheme (`#FFC107` yellow on white) is decorative but chart legends inherit the same colours and may fail.
- ⚠️ **Alt Text** — No images; the two `<canvas>` chart surfaces (`index.html:35-36`) have no `aria-label` or visually hidden text summary, so screen-reader users get nothing from the primary output of the calculator.
- ❌ **Focus Indicators** — `styles.css` does not define `:focus` or `:focus-visible` styles. Browser defaults apply, but `button { border: none; ... }` removes the default outline on some browsers without restoring a visible focus ring.
- ❌ **Reduced Motion** — Chart.js animates by default; no `prefers-reduced-motion` handling and no Chart.js `animation: false` override.

### Tier 1: SEO Fundamentals

- ❌ **Semantic HTML for Search** — `<title>Break-Even Calculator</title>` is generic. No `<meta name="description">`, no structured heading hierarchy beyond a single `<h1>`.
- ❌ **Open Graph & Social Tags** — None present (`og:title`, `og:description`, `og:image`, `twitter:card` all missing).
- ❌ **Structured Data** — No JSON-LD (could meaningfully use `SoftwareApplication` or `FAQPage` schema for a calculator).
- ❌ **Technical SEO** — No `robots.txt`, no `sitemap.xml`, no `<link rel="canonical">`.
- ❌ **Performance as SEO** — No Core Web Vitals tracking, no Lighthouse CI baseline.

### Tier 1: SEO Operations *(v4 addition)*

- ❌ **Google Search Console** — No GSC verification meta tag; no evidence of property registration.
- ❌ **Bing Webmaster Tools + IndexNow** — No IndexNow key file at site root.
- ❌ **Programmatic IndexNow Pings** — N/A without IndexNow set up; no `@getdigitalos/seo` integration.
- ❌ **Pre-Launch Validation Gate** — No record of Rich Results Test run.
- ❌ **Canonical Implementation** — `@getdigitalos/seo` not used (no `package.json` to host it). No ADR explaining the deviation.

### Tier 1: Observability & Diagnostics *(v4 addition)*

- ❌ **Privacy Analytics** — No Plausible script. `NEXT_PUBLIC_PLAUSIBLE_DOMAIN` is meaningless in a plain HTML project, but the principle (cookieless analytics) is unmet — there are no analytics at all.
- ❌ **Pre-Launch DNS + Email Diagnostic Gate** — N/A for email but the project does have a custom-domain expectation per registry stack profile; no MXToolbox / Workspace Toolbox run logged in `launch-checklist.md` (no such file exists).

### Tier 1: Code Quality

- ✅ **Separation of Concerns** — Structure (`index.html`), presentation (`styles.css`), behaviour (`script.js`) are properly separated.
- ⚠️ **DRY Principle** — `renderBarChart` and `renderLineChart` share the destroy-before-recreate pattern but are otherwise fine for the size. The break-even formula `totalExpenses / (1 - rate)` is duplicated in two loops (`script.js:26` and `:55`) — minor.
- ✅ **Semantic Naming** — Variable names (`childcareCost`, `householdTaxRate`, `breakEvenIncomes`) clearly describe purpose.
- ❌ **Mobile-First CSS** — Base styles target a 400px-wide desktop card with `height: 100vh` centring (`styles.css:11`). On mobile this clips content. There are no `min-width` queries enhancing from a mobile base.

### Tier 1: DevOps Basics

- ✅ **Automated Deployment** — Deploys to Cloudflare Pages via git push (per registry `stack.deploy = ["cloudflare-pages"]`).
- ⚠️ **Preview Deployments** — Cloudflare Pages provides preview URLs per branch by default; not verified that they're enabled, but no opt-out config exists either.
- ⚠️ **Environment Separation** — No explicit staging environment is configured. Cloudflare Pages preview branches serve as de-facto staging.
- ⚠️ **Domain & DNS Management** — DNS not documented in this repo. SSL auto-renews via Cloudflare.
- ✅ **Version Control** — Git used; main is the deployable branch. Commit history is shallow (a long run of `"Add files via upload"` and one literal `"Update ,"`) but it exists.

### Tier 1: UX Fundamentals

- ❌ **Responsive Design** — Fixed 400px container; not tested across breakpoints.
- ⚠️ **Error Prevention** — Validates `isNaN` and shows `alert("Please fill in all fields with valid numbers.")` (`script.js:18`). `alert()` is a poor UX pattern: blocks the page, no inline error highlight, no recovery path on the offending field.
- ❌ **Loading States** — Calculate is synchronous so technically instant, but there is no textual result anywhere — only charts. A user with a screen reader or who simply mistrusts the chart has no readable break-even number.
- ✅ **Consistent Patterns** — All five inputs follow the same `<label> + <input type="number">` pattern.

### Cross-Cutting: Privacy & Data Protection (Tier 1)

- ❌ **Privacy policy** — None. Although no PII is collected by the app, the page does load `cdn.jsdelivr.net`, which observes user IP/UA. Per the v2 Privacy Checklist, even loading third-party assets warrants a brief disclosure.
- ❌ **Cookie disclosure** — None. (Chart.js / jsDelivr do not set cookies, but no statement attests to this.)
- ✅ **Secure form submission** — No form submission; calculation is local-only.
- ✅ **Data minimization** — Excellent: no data is collected or transmitted.

### Cross-Cutting: AI/LLM Integration

⬜ **Not Applicable.** Project has no AI features and the classification confirms it.

### Cross-Cutting: Dependency Management

- ❌ **Lock file committed** — No `package.json` or `package-lock.json`. The single runtime dependency (Chart.js) is loaded as an unpinned CDN URL, which is the worst case for reproducibility — every page load gets whatever `latest` happens to be that day.
- ❌ **`npm audit` clean** — Cannot run; no package manifest. SCA tooling has nothing to scan.
- ⚠️ **Dependency evaluation** — Chart.js is a reasonable choice (MIT, actively maintained, large community), but the *delivery* method (unpinned CDN, no SRI) bypasses the evaluation entirely.

### Cross-Cutting: Project Hub Registration

- ✅ **Registered in Project Hub** — Entry exists at `C:/dev/project-hub/registry/projects.json:1595`.
- ✅ **Tier matches classification** — Fixed during this audit. Registry was `"unclassified"`; now `"1"`, matching `PROJECT_CLASSIFICATION.md`.
- ✅ **Git remote configured** — `https://github.com/GetDigitalOS/EconomicValueOfSAHS.git`.
- ⚠️ **Dev port assigned** — No `dev_port` field in this project's registry entry. Acceptable for a static-only project with no local dev server, but worth confirming.
- ✅ **Status is current** — `last_synced` was 2026-05-24, `last_audited` set to 2026-05-23 by this run.

## Recommendations (Prioritized)

### 🔴 Critical (Security/Compliance Risk)

1. **Add a `_headers` file at repo root** for Cloudflare Pages with a CSP that allow-lists `'self'` and the pinned jsDelivr URL, plus `Strict-Transport-Security`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, and `X-Frame-Options: DENY`. Single file, one deploy, eliminates a whole class of attacks.
2. **Pin Chart.js to a specific version and add Subresource Integrity.** Replace `index.html:9` with something like `<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.6/dist/chart.umd.min.js" integrity="sha384-..." crossorigin="anonymous" defer></script>`. Better still: download Chart.js into the repo and serve it from `/vendor/chart.umd.min.js` so the page has no third-party runtime dependency at all.
3. **Add the SEO baseline** in `index.html`: `<meta name="description">`, canonical `<link>`, Open Graph + Twitter Card tags, a `robots.txt`, and a one-URL `sitemap.xml`. Registry has `seo: "required"` — this is non-optional for the tier.

### 🟡 Important (Quality/Reliability Risk)

4. **Make the layout actually responsive.** Replace `.container { width: 400px }` with `width: min(400px, 100% - 32px)` (or similar) and remove the `height: 100vh` flex centring that breaks on tall content. Add one breakpoint at ~640px to widen the card on tablet+.
5. **Render a readable result, not just charts.** Insert a `<div role="status" aria-live="polite">` that displays the computed break-even income, household tax, and service tax in plain text. This fixes accessibility, gives keyboard / screen-reader users access to the output, and protects against silent Chart.js failure.
6. **Label the canvases**: add `aria-label="Bar chart of cost breakdown"` to `#bar-chart` and `aria-label="Line chart of break-even income vs. tax rate"` to `#line-chart` (`index.html:35-36`).
7. **Replace `alert()` with inline validation.** Highlight the offending field(s) with an error state and an `aria-describedby` error message. Validate ranges (no negative costs; tax rate between 0–100).
8. **Add a visible `:focus-visible` style** on the Calculate button and inputs in `styles.css`. The current `button { border: none }` plus no focus rule violates a Tier 1 accessibility checkbox.
9. **Honour `prefers-reduced-motion`** by passing `options: { animation: false }` to both Chart.js constructors when `window.matchMedia('(prefers-reduced-motion: reduce)').matches`.
10. **Add a minimal privacy notice** (a paragraph in the footer or a separate `/privacy.html`) stating that no data is collected, no cookies are set, and that Chart.js is loaded from jsDelivr.

### 🟢 Recommended (Best Practice)

11. **Establish a token layer** even though the project is tiny: define `--color-brand`, `--color-text`, `--space-1` through `--space-4`, and reuse them. The cost is ~10 lines of CSS and unlocks dark mode / theming if ever needed.
12. **Strip `console.log` debug calls** from `script.js:38-42`.
13. **Add a `package.json`** with `devDependencies` for a minifier (`terser`, `lightningcss`) and an `npm audit` script in CI. This is what makes the Dependency Hygiene checkbox actually checkable.
14. **Generate the Tier 1 documentation stack** (CLAUDE.md, design-tokens.md, site-spec.md, system-architecture.md, plus a tier ADR). The `docs/` directory exists but contains only this report and PROJECT_CLASSIFICATION.md. Use `/setup-docs` to scaffold the rest.
15. **Tighten commit hygiene.** The history has 12 consecutive `"Add files via upload"` commits and one literal `"Update ,"`. Future commits should use Conventional Commits or at minimum be descriptive.

## Next Review

Recommended next compliance check: **2026-08-23** (90 days), or sooner if any of the 🔴 critical items are addressed and you want to re-baseline.
