# Universal Web Development Principles v2

| Field | Value |
|-------|-------|
| **Version** | v2.02.01 |
| **Date** | 2026-03-22 |
| **Status** | Locked |
| **Applies To** | All Tier 1–4 web projects across the portfolio |
| **Owner** | @getdigital2020 |
| **Supersedes** | Universal Web Development Principles v1 |

### Changelog

| Version | Date | Changes |
|---------|------|---------|
| v2.02.01 | 2026-03-22 | Added Cross-Cutting Concern: Structural Integrity — codifies the principle that solutions must address root causes at the correct structural level. No workarounds, no manual sync steps, no swallowed errors. Includes anti-patterns table, decision test, and tier-specific checklist items. Added 9 entries to Quick Reference Matrix. |
| v2.01.02 | 2026-03-20 | Patch: fixed footer version mismatch (was "Version 2.0", now matches header), replaced placeholder owner/review-date in footer with actual values, set next quarterly review date. |
| v2.01.01 | 2026-03-17 | Migrated to canonical/references/ as single source of truth. Applied versioning standard header. Removed scattered copies from individual projects — all projects now point here. No content changes from v2.0. |
| v2.0 | 2026-02-27 | Added: AI/LLM integration section, tier escalation rules, expanded design system module, SEO section, frontend architecture patterns, error/edge case UX patterns, DevOps for Tiers 1–2, dependency management philosophy, technology selection principles, content management guidance, enhanced testing section, pre-Tier 3 observability. Revised: UX Behavior Settings (added accessibility warnings), classification system (weighted scoring + auto-escalation). Removed: None (all v1 content preserved and enhanced). |
| v1.0 | 2025 | Initial release. |

### Review Cadence & Governance

This document should be reviewed quarterly. Proposed changes should include rationale and be reviewed by at least one other team member before merging. Major changes (new tiers, removed principles) require an ADR (Architecture Decision Record). The document owner is responsible for triggering quarterly reviews and maintaining the changelog.

---

## Project Classification

Before applying development principles, classify this project:

### Complexity Assessment Questions

1. **User Authentication**: Does the app require user accounts, login, or personalized data?
2. **Data Persistence**: Is there a database with user-generated content that must be stored/retrieved?
3. **Multi-Role Access**: Are there different user types with different permissions (admin, user, viewer)?
4. **Third-Party Integrations**: Does it connect to external APIs, payment processors, or services?
5. **Real-Time Features**: Are there live updates, websockets, or collaborative features?
6. **Transaction Sensitivity**: Does it handle payments, PII, health data, or financial information?
7. **Scale Expectations**: Will this serve >10,000 users or handle concurrent heavy load?
8. **Team Size**: Will multiple developers work on this codebase?
9. **Longevity**: Is this expected to be maintained/extended for 2+ years?
10. **AI/LLM Features**: Does the app use AI models for generation, classification, or recommendations? *(New in v2)*

### Tier Auto-Escalation Rules *(New in v2)*

Before counting "yes" answers, apply these override rules. Certain answers automatically set a minimum tier regardless of total count:

| If This Is "Yes"... | Minimum Tier | Rationale |
|----------------------|:------------:|-----------|
| **Transaction Sensitivity** | Tier 3 | Payments, PII, and health data demand authentication architecture, audit logging, encryption at rest, and compliance frameworks that only exist at Tier 3+. A two-page site collecting credit card numbers is not a "Tier 1" project. |
| **Multi-Role Access** | Tier 3 | RBAC requires API-level authorization enforcement, which assumes an application architecture. |
| **Real-Time Features** | Tier 3 | WebSockets, pub/sub, and state synchronization require application-level infrastructure. |
| **AI/LLM Features** (with user-facing output) | Tier 3 | Prompt injection defense, output guardrails, and non-deterministic testing require application-level rigor. |

After applying escalation rules, count remaining "yes" answers to determine if the project should be in a *higher* tier than the escalated minimum.

### Classification Tiers

| Tier | Profile | Typical Examples |
|------|---------|------------------|
| **Tier 1: Static/Marketing** | 0-1 "yes" answers (no escalation triggers) | Brochure sites, landing pages, blogs |
| **Tier 2: Interactive** | 2-3 "yes" answers (no escalation triggers) | Sites with forms, calculators, basic CMS, contact management |
| **Tier 3: Application** | 4-6 "yes" answers, OR any escalation trigger | SaaS products, dashboards, authenticated portals |
| **Tier 4: Platform** | 7+ "yes" answers | Multi-tenant systems, marketplaces, financial platforms |

---

## Tier 1: Static/Marketing Site Principles

*Apply these to all projects regardless of complexity.*

### Security Fundamentals
- [ ] **HTTPS Everywhere** – TLS for all traffic, no mixed content
- [ ] **Security Headers** – HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, CSP
- [ ] **Input Validation** – Sanitize all form inputs, even on "simple" contact forms
- [ ] **Dependency Hygiene** – Minimal dependencies, keep updated, no known vulnerabilities (run `npm audit` or equivalent on every build)

### Design System Basics
- [ ] **Semantic Tokens** – Colors, spacing, typography defined by purpose not value
- [ ] **Token Hierarchy** – Primitives → Semantic → Component (even if simple)
- [ ] **Consistent Spacing Scale** – 4px or 8px base unit, consistent multipliers
- [ ] **Typography Scale** – Defined hierarchy (not arbitrary font sizes)
- [ ] **Icon System** – Consistent icon library selected (Lucide, Phosphor, Heroicons); all icons include `aria-label` or are marked `aria-hidden="true"` if decorative *(New in v2)*
- [ ] **Responsive Breakpoint Strategy** – Defined breakpoints (e.g., 640/768/1024/1280px); fluid typography using `clamp()`; container queries for component-level responsiveness where supported *(New in v2)*

### Performance
- [ ] **Core Web Vitals Targets** – LCP <2.5s, INP <200ms, CLS <0.1
- [ ] **Asset Optimization** – Compressed images (WebP/AVIF), minified CSS/JS
- [ ] **Lazy Loading** – Defer offscreen images and non-critical resources
- [ ] **CDN Delivery** – Static assets served from edge locations

### Accessibility (WCAG 2.1 AA Minimum)
- [ ] **Semantic HTML** – Proper heading hierarchy, landmarks, button vs. link distinction
- [ ] **Keyboard Navigation** – All interactive elements focusable and operable
- [ ] **Color Contrast** – 4.5:1 for text, 3:1 for UI components
- [ ] **Alt Text** – Meaningful descriptions for images
- [ ] **Focus Indicators** – Visible focus states for all interactive elements
- [ ] **Reduced Motion** – Respect `prefers-reduced-motion` media query for all animations *(New in v2)*

### SEO Fundamentals *(New in v2)*
- [ ] **Semantic HTML for Search** – Proper heading hierarchy, structured content, meaningful `<title>` and `<meta description>` on every page
- [ ] **Open Graph & Social Tags** – `og:title`, `og:description`, `og:image`, `twitter:card` meta tags for social sharing
- [ ] **Structured Data** – JSON-LD for organization, breadcrumbs, FAQ, or article schema as appropriate
- [ ] **Technical SEO** – `robots.txt`, XML sitemap, canonical URLs on all pages, proper 301 redirects
- [ ] **Performance as SEO** – Core Web Vitals directly impact search ranking; connect performance work to SEO outcomes

### Code Quality
- [ ] **Separation of Concerns** – Structure (HTML), presentation (CSS), behavior (JS)
- [ ] **DRY Principle** – Reusable components, no copy-paste code blocks
- [ ] **Semantic Naming** – Classes/variables describe purpose, not appearance
- [ ] **Mobile-First CSS** – Base styles for mobile, progressively enhance

### DevOps Basics *(New in v2)*
- [ ] **Automated Deployment** – Deploy via Git push (Vercel, Netlify, Cloudflare Pages, or equivalent); no manual FTP/SSH deploys
- [ ] **Preview Deployments** – Every pull request gets a preview URL for review before merge
- [ ] **Environment Separation** – At minimum: production and staging/preview; no testing directly in production
- [ ] **Domain & DNS Management** – DNS records documented; SSL certificate auto-renewing
- [ ] **Version Control** – Git with meaningful commit messages; main branch always deployable

### UX Fundamentals
- [ ] **Responsive Design** – Fluid layouts, tested across breakpoints
- [ ] **Error Prevention** – Form validation before submission, clear labels
- [ ] **Loading States** – Visual feedback during any async operations
- [ ] **Consistent Patterns** – Same interaction = same behavior throughout

---

## Tier 2: Interactive Site Principles

*Apply Tier 1 + these principles for sites with forms, calculators, and basic dynamic content.*

### Enhanced Security
- [ ] **CSRF Protection** – Token validation on all form submissions
- [ ] **Rate Limiting** – Prevent form spam and abuse
- [ ] **Honeypot Fields** – Non-visible fields to catch bots
- [ ] **Output Encoding** – Prevent XSS when displaying any user-provided data
- [ ] **Content Security Policy** – Restrictive CSP, no inline scripts where possible
- [ ] **Dependency Scanning** – Automated vulnerability detection integrated into build process (e.g., `npm audit`, Snyk, or Dependabot) *(Moved from Tier 4 in v2 — even Tier 2 sites with npm dependencies need this)*

### Data Handling
- [ ] **Client-Side Validation + Server-Side Validation** – Never trust client alone
- [ ] **Data Minimization** – Collect only what's needed
- [ ] **Secure Transmission** – Form data over HTTPS, sensitive fields not in URLs
- [ ] **Privacy Compliance** – Cookie consent, privacy policy, data handling disclosure

### Design System Extension
- [ ] **Component Token Layer** – Button, input, card-specific tokens
- [ ] **Interactive State Tokens** – Hover, focus, active, disabled states defined
- [ ] **Motion Design Principles** – Consistent animation durations (150ms micro-interactions, 300ms transitions, 500ms page-level); consistent easing (ease-out for entrances, ease-in for exits); all motion respects `prefers-reduced-motion` *(Expanded in v2)*
- [ ] **Form Component Library** – Consistent inputs, selects, checkboxes, validation states
- [ ] **Content Design Standards** – Defined patterns for error messages, empty states, confirmation dialogs, loading copy, and success messages; consistent tone and voice *(New in v2)*

### Frontend Architecture Basics *(New in v2)*
- [ ] **State Ownership** – Each piece of state has a clear owner: URL state (shareable, bookmarkable), component state (ephemeral UI), or server state (fetched, cached, synced)
- [ ] **Data Fetching Strategy** – Defined approach to loading data: fetch-on-render vs. fetch-then-render; loading/error/success states handled consistently
- [ ] **Rendering Strategy Decision** – Conscious choice of CSR, SSR, SSG, or ISR based on content type and update frequency; documented rationale

### Architecture
- [ ] **Separation of Concerns** – Business logic separate from presentation
- [ ] **Configuration Externalized** – No hardcoded URLs, API keys, or environment-specific values
- [ ] **Error Handling Strategy** – Consistent error display, meaningful messages, graceful fallbacks
- [ ] **State Management** – Clear ownership of state (URL, local, component)

### Error & Edge Case UX *(New in v2)*
- [ ] **Empty States** – Designed screens for when there's no data yet (first use, no results, cleared data); include guidance or CTAs
- [ ] **Partial Failure** – When some data loads and some doesn't, show what succeeded with clear indication of what failed; offer retry
- [ ] **Timeout UX** – After defined thresholds (e.g., 3s), show progress indicator; after longer threshold (e.g., 10s), offer cancel/retry
- [ ] **Form Error Recovery** – Never clear user input on error; preserve state and highlight what needs fixing
- [ ] **Offline Awareness** – At minimum, detect loss of connectivity and inform the user before they attempt actions that will fail

### Performance Enhancement
- [ ] **Code Splitting** – Load calculator/form logic only when needed
- [ ] **Debouncing/Throttling** – Input handlers that fire appropriately (not on every keystroke)
- [ ] **Request Cancellation** – Abort superseded requests (AbortController)
- [ ] **Caching Strategy** – Static assets cached aggressively, API responses cached appropriately

### Testing
- [ ] **Form Validation Testing** – All validation rules covered
- [ ] **Calculator Logic Testing** – Unit tests for computational logic
- [ ] **Cross-Browser Testing** – Functionality verified across target browsers
- [ ] **Accessibility Testing** – Automated (axe-core in CI) + manual screen reader testing (VoiceOver on Mac, NVDA on Windows; test key flows, not just pages) *(Expanded in v2)*
- [ ] **Visual Regression Testing** – Screenshot comparison for UI changes (Chromatic, Percy, or Playwright visual comparisons) *(New in v2)*

### Observability *(New in v2 — moved from Tier 3 with lighter requirements)*
- [ ] **Client-Side Error Capture** – Unhandled exceptions and promise rejections reported to a logging service (Sentry, LogRocket, or equivalent)
- [ ] **Source Maps in Production** – Uploaded to error tracking service (not publicly served) so minified stack traces are readable
- [ ] **Basic Analytics** – Page views, form submissions, and key user actions tracked (privacy-respecting: Plausible, Fathom, or consent-gated Google Analytics)
- [ ] **Uptime Monitoring** – Alerts if site becomes unavailable

### Content Management *(New in v2)*
- [ ] **CMS Evaluation** – If non-developers need to update content, select an appropriate CMS; prefer headless CMS (Sanity, Contentful, Strapi) for developer flexibility with editor-friendly interfaces
- [ ] **Content Modeling** – Structured content types defined (not free-form WYSIWYG blobs); content is data, not layout
- [ ] **Editorial Preview** – Content editors can preview changes before publishing
- [ ] **Content Versioning** – Ability to roll back content changes

---

## Tier 3: Application Principles

*Apply Tier 1 + Tier 2 + these principles for authenticated apps with persistent data.*

### Security (Critical)
- [ ] **Authentication Architecture** – OAuth 2.0/OIDC with appropriate flow (PKCE for SPAs)
- [ ] **Session Management** – Secure cookies, appropriate timeouts, rotation on privilege change
- [ ] **RBAC Implementation** – Roles defined, permissions enforced at API level (not just UI)
- [ ] **Principle of Least Privilege** – Users/services get minimum required access
- [ ] **Secrets Management** – No secrets in code, use environment injection or vault
- [ ] **JWT Best Practices** – Short expiry, refresh tokens, no sensitive data in payload, proper validation
- [ ] **Parameterized Queries** – No SQL injection vectors
- [ ] **API Authentication** – All endpoints authenticated, no security through obscurity
- [ ] **Audit Logging** – Who did what, when (immutable)
- [ ] **Security Testing** – SAST (static analysis) integrated into CI; DAST (dynamic analysis) run periodically against staging; dependency scanning blocks deploys on critical vulnerabilities *(New in v2)*

### AI/LLM Integration Security *(New in v2)*
- [ ] **Prompt Injection Defense** – All user input that flows into LLM prompts is treated as untrusted; use structured prompt templates with clear system/user boundaries; never concatenate raw user input into system prompts
- [ ] **Output Guardrails** – LLM outputs are validated and sanitized before display or use in application logic; define and enforce output schemas where possible (JSON mode, function calling)
- [ ] **Content Filtering** – Apply safety filters on both input and output; define what categories of content are out of scope for your application
- [ ] **Fallback Behavior** – Define what happens when AI service is unavailable, rate-limited, or returns unexpected output; never let an AI failure crash the application
- [ ] **Cost Controls** – Token budgets per request and per user; caching of identical or similar requests; monitoring of spend with alerts

### Database & Data
- [ ] **ACID Compliance** – Transactions where data integrity matters
- [ ] **Referential Integrity** – Foreign keys enforced at database level
- [ ] **Indexing Strategy** – Queries analyzed, appropriate indexes created
- [ ] **Migration Strategy** – Version-controlled, reversible schema changes
- [ ] **Soft Deletes** – Where audit/recovery requirements exist
- [ ] **Backup & Recovery** – Defined RPO/RTO, tested restores
- [ ] **Connection Pooling** – Managed connections, pool limits understood

### Architecture (SOLID + 12-Factor)
- [ ] **Single Responsibility** – Each module/service does one thing
- [ ] **Open/Closed Principle** – Extend behavior without modifying existing code
- [ ] **Dependency Inversion** – Depend on abstractions, inject dependencies
- [ ] **Stateless Processes** – No server-side session state (enables scaling)
- [ ] **Config in Environment** – Environment variables, not hardcoded values
- [ ] **Backing Services as Attachments** – Database, cache, queue are swappable resources
- [ ] **Dev/Prod Parity** – Minimize environment drift
- [ ] **API-First Design** – Contracts defined before implementation
- [ ] **API Versioning** – Strategy for backward compatibility

### Frontend Architecture *(New in v2)*
- [ ] **State Management Architecture** – Explicit choice documented: server state (React Query/SWR/TanStack Query) vs. client state (Zustand, Jotai, Context, Redux) vs. URL state (search params, path); each state type uses the appropriate tool
- [ ] **Data Fetching Patterns** – Stale-while-revalidate for read-heavy data; optimistic updates for user actions; cache invalidation strategy defined; loading/error/empty states for every async boundary
- [ ] **Rendering Strategy** – SSR for SEO-critical pages, CSR for authenticated dashboards, SSG for static content, streaming SSR for large pages; documented rationale for each route
- [ ] **Error Boundaries** – React error boundaries (or framework equivalent) at key UI boundaries; graceful degradation shows partial UI rather than full-page crash
- [ ] **API Layer Abstraction** – API calls centralized through a client layer (not scattered `fetch` calls); consistent error handling, auth token injection, retry logic

### Design System Maturity
- [ ] **Full Token Architecture** – Primitives, semantic, component tokens complete
- [ ] **Theming Support** – Dark mode, white-label via token swapping
- [ ] **Design-Dev Sync** – Figma variables = code tokens
- [ ] **Component API Standards** – Consistent prop naming conventions (e.g., `variant`, `size`, `disabled`); composition over configuration; polymorphic components use `as` prop; all components accept `className` for escape hatches *(Expanded in v2)*
- [ ] **Documentation** – Component usage, do's and don'ts
- [ ] **Icon System Governance** – Single icon source of truth; custom icons follow same grid/stroke/naming as library icons; icon components accept `size` and `color` props from tokens *(New in v2)*

### Error & Edge Case UX (Application-Level) *(New in v2)*
- [ ] **Conflict Resolution UX** – When two users edit the same resource, detect and present conflicts clearly; offer merge, overwrite, or discard options
- [ ] **Rate Limit UX** – When the user hits API rate limits, show clear feedback with estimated wait time; queue actions if possible rather than dropping them
- [ ] **Offline-First Patterns** – For mobile-heavy apps: service worker caching of critical assets; optimistic UI for writes with background sync; clear online/offline status indicators
- [ ] **Undo/Redo** – For destructive actions, offer undo window (soft delete + toast with undo button) rather than confirmation dialogs where possible
- [ ] **Session Expiration** – When auth session expires mid-use, preserve the user's in-progress work; re-authenticate and resume rather than losing state

### AI/LLM Application Patterns *(New in v2)*
- [ ] **Prompt Management** – Prompts stored as versioned templates, not inline strings; variables injected at runtime; prompt changes deployable without code changes where possible
- [ ] **Evaluation Framework** – Non-deterministic outputs tested via evaluation suites (not traditional unit tests); define rubrics for quality, safety, and relevance; run evals on prompt changes
- [ ] **Streaming UX** – For generative AI responses, stream tokens to the UI; show typing indicator; allow user to cancel mid-generation
- [ ] **Transparency & Disclosure** – Clearly indicate AI-generated content to users; provide confidence signals where appropriate; don't present AI output as human-authored
- [ ] **Data Privacy with AI Providers** – Document what user data flows to third-party AI APIs; ensure DPAs are in place; offer opt-out where feasible; never send PII to AI providers without explicit consent and necessity
- [ ] **Caching & Deduplication** – Cache identical AI requests (semantic or exact match); deduplicate concurrent identical requests; significant cost and latency savings

### Resilience
- [ ] **Graceful Degradation** – Reduced functionality beats total failure
- [ ] **Circuit Breaker** – Prevent cascade failures from downstream services
- [ ] **Retry with Backoff** – Transient failure handling
- [ ] **Timeout Configuration** – No unbounded waits
- [ ] **Health Checks** – Liveness and readiness endpoints
- [ ] **Graceful Shutdown** – Drain connections, complete in-flight requests

### Concurrency
- [ ] **Idempotency Keys** – Safe retries for critical operations (payments, submissions)
- [ ] **Optimistic Locking** – Version checks for concurrent edits
- [ ] **Race Condition Awareness** – Identified and mitigated in critical paths
- [ ] **Distributed Locks** – Where required for cross-instance coordination

### Testing Maturity
- [ ] **Testing Pyramid** – Unit (many) > Integration (some) > E2E (few)
- [ ] **Test Isolation** – No shared state, deterministic, order-independent
- [ ] **Test Data Factories** – Consistent, isolated test data generation
- [ ] **CI Integration** – Tests run on every commit, block on failure
- [ ] **Coverage Thresholds** – Enforced minimums for critical paths
- [ ] **Load & Performance Testing** – Baseline response times established; load tests run against staging before major releases; results compared against baselines *(New in v2)*
- [ ] **Contract Testing** – API contracts validated between frontend and backend (Pact, or OpenAPI schema validation); breaking changes caught before deployment *(New in v2)*
- [ ] **Security Testing in CI** – SAST scanning on every PR; dependency vulnerability checks block merges for critical/high severity *(New in v2)*
- [ ] **Accessibility Testing Protocol** – axe-core integrated into CI (zero violations on new code); manual screen reader testing (VoiceOver + NVDA) for key user flows before each release; keyboard-only navigation tested for all new features *(New in v2)*

### Observability
- [ ] **Structured Logging** – JSON format, consistent fields, correlation IDs
- [ ] **Metrics Collection** – Request latency, error rates, business metrics
- [ ] **Distributed Tracing** – Request flow across services
- [ ] **Alerting on Symptoms** – User-impacting signals, not just system metrics
- [ ] **Dashboards** – Key metrics visualized, accessible to team
- [ ] **Real User Monitoring (RUM)** – Client-side performance data collected from real users (not just synthetic tests); tied to Core Web Vitals *(New in v2)*
- [ ] **Session Replay** – Available for debugging user-reported issues; privacy implications documented and user consent obtained; PII masking configured *(New in v2)*

### Operational Maturity
- [ ] **CI/CD Pipeline** – Automated build, test, deploy
- [ ] **Infrastructure as Code** – Reproducible environments
- [ ] **Feature Flags** – Decouple deployment from release
- [ ] **Rollback Capability** – Quick revert on issues
- [ ] **Runbooks** – Documented incident response

### Internationalization (if applicable)
- [ ] **i18n Architecture** – String extraction, ICU message format
- [ ] **UTC Storage** – Timestamps in UTC, convert at display
- [ ] **Locale-Aware Formatting** – Dates, numbers, currency via Intl API

---

## Tier 4: Platform Principles

*Apply Tier 1 + Tier 2 + Tier 3 + these principles for multi-tenant, high-scale, or regulated systems.*

### Security (Maximum)
- [ ] **Zero Trust Architecture** – No implicit trust, verify everything
- [ ] **Defense in Depth** – Multiple security layers, no single point of failure
- [ ] **ABAC Consideration** – Attribute-based access for complex permission models
- [ ] **Tenant Isolation** – Data separation enforced at all layers
- [ ] **Secrets Rotation** – Automated, regular rotation of all credentials
- [ ] **Penetration Testing** – Regular third-party security assessments
- [ ] **Dependency Scanning** – Automated vulnerability detection in CI
- [ ] **Security Incident Response Plan** – Documented, practiced

### AI/LLM Platform Patterns *(New in v2)*
- [ ] **Multi-Model Strategy** – Abstraction layer supporting multiple LLM providers; no hard coupling to a single vendor; model selection per use case (cost vs. quality vs. latency)
- [ ] **AI Observability** – Token usage, latency, error rates, and cost tracked per model, per feature, and per tenant; anomaly detection on AI spend
- [ ] **Prompt Versioning & Rollback** – Prompts version-controlled with rollback capability; A/B testing infrastructure for prompt variants
- [ ] **User Feedback Loop** – Thumbs up/down or rating on AI outputs; feedback data feeds into evaluation and fine-tuning pipelines
- [ ] **Abuse Prevention** – Rate limiting per user on AI features; detection of prompt injection attempts; automated flagging of adversarial usage patterns
- [ ] **Regulatory Compliance for AI** – Document AI decision-making processes for features that affect users materially; support human review override; maintain audit trail of AI inputs and outputs for regulated industries

### Database (Scale)
- [ ] **Read/Write Separation** – Replicas for read scaling
- [ ] **Sharding Strategy** – If single-database limits approached
- [ ] **Zero-Downtime Migrations** – Expand/contract pattern, backward-compatible changes
- [ ] **Slow Query Monitoring** – Identify and address before user impact
- [ ] **Data Classification** – Categorize by sensitivity, handle accordingly
- [ ] **Data Residency** – Compliance with geographic storage requirements

### Architecture (Distributed Systems)
- [ ] **CAP Theorem Awareness** – Explicit tradeoff decisions documented
- [ ] **Eventual Consistency Handling** – Where applied, UI reflects appropriately
- [ ] **Saga Pattern** – Distributed transaction management
- [ ] **Event Sourcing** – Where audit/replay requirements exist
- [ ] **CQRS** – Separate read/write models where beneficial
- [ ] **Outbox Pattern** – Reliable event publishing with database writes
- [ ] **Dead Letter Queues** – Failed async job handling and diagnosis
- [ ] **Bulkhead Pattern** – Isolate failures, prevent system-wide impact
- [ ] **Backend for Frontend** – Dedicated APIs per client type if needed
- [ ] **Strangler Fig** – Incremental migration strategy for legacy

### Operational Excellence
- [ ] **SLOs/SLIs/SLAs** – Defined, measured, reported
- [ ] **Error Budgets** – Policy for reliability vs. velocity tradeoff
- [ ] **Chaos Engineering** – Proactive failure testing
- [ ] **Blameless Postmortems** – Learning from incidents
- [ ] **On-Call Rotation** – Defined, sustainable, documented
- [ ] **Capacity Planning** – Proactive scaling, not reactive
- [ ] **Blue/Green or Canary Deploys** – Safe rollout strategies
- [ ] **Multi-Region Capability** – If uptime requirements demand

### Compliance & Governance
- [ ] **Privacy by Design** – Baked in, not bolted on
- [ ] **Consent Management** – Granular opt-in/opt-out
- [ ] **Right to Deletion** – Technical capability to purge user data
- [ ] **Data Retention Policies** – Defined, enforced, auditable
- [ ] **Audit Trail** – Comprehensive, immutable, queryable
- [ ] **Change Management** – Controlled, documented, approved changes
- [ ] **ADRs (Architecture Decision Records)** – Document why decisions were made
- [ ] **Compliance Certifications** – SOC 2, HIPAA, PCI-DSS as required

### Process & Collaboration
- [ ] **Semantic Versioning** – MAJOR.MINOR.PATCH with meaning
- [ ] **Conventional Commits** – Structured commit messages
- [ ] **Trunk-Based Development** – Short-lived branches, continuous integration
- [ ] **Code Review Standards** – Defined criteria, enforced
- [ ] **API Contract Testing** – Pact or similar for service boundaries
- [ ] **Documentation Standards** – Architecture diagrams, API docs, runbooks current

---

## Cross-Cutting Concern: AI/LLM Integration *(New in v2)*

*AI isn't a tier — it's a cross-cutting concern like privacy. A Tier 1 site with an AI chatbot widget has AI obligations. Apply the relevant principles based on how you use AI, not just your tier.*

### Why This Section Exists

AI/LLM integration introduces a category of concerns that doesn't map neatly to the traditional web development stack: non-deterministic outputs that can't be unit tested traditionally, prompt injection as a new attack vector comparable to SQL injection, cost models that scale with usage in unfamiliar ways, and privacy implications of sending user data to third-party model providers. These concerns apply whether you're adding a chatbot to a marketing site or building an AI-native SaaS platform.

### AI Integration by Tier

| Tier | Typical AI Usage | Key Concerns |
|------|-----------------|--------------|
| **Tier 1** | Embedded chatbot widget, AI-generated content (static) | Disclosure of AI-generated content; widget's data practices in privacy policy |
| **Tier 2** | AI-powered form assistance, smart search, content recommendations | Input sanitization before AI processing; cost controls; fallback when AI service is down |
| **Tier 3** | AI features in authenticated app (suggestions, classification, generation) | Prompt injection defense; output guardrails; evaluation framework; user data privacy with AI providers; streaming UX |
| **Tier 4** | AI as core platform capability; multi-tenant AI features; AI decisions affecting users materially | Multi-model abstraction; per-tenant cost tracking; AI observability; regulatory compliance; abuse prevention; audit trails for AI decisions |

### Prompt Injection — The New SQL Injection

Prompt injection occurs when user-provided content manipulates the behavior of an LLM by being interpreted as instructions rather than data. This is analogous to SQL injection and should be treated with equivalent seriousness.

**Defense patterns:**
- Structured prompt templates with clear system/user delimiters
- Input validation and sanitization before LLM processing
- Output validation against expected schemas
- Never place raw user input in system prompts
- Use tool/function calling to constrain output format
- Defense in depth: multiple layers, not a single filter

### Cost Management Framework

AI API costs can grow unpredictably. Establish controls:

| Control | Implementation |
|---------|---------------|
| **Per-request budget** | Set `max_tokens` appropriate to the task, not a blanket high number |
| **Per-user rate limits** | Prevent individual users from generating excessive cost |
| **Caching** | Cache identical requests; consider semantic similarity caching for near-duplicates |
| **Model tiering** | Use cheaper/faster models for simple tasks; reserve expensive models for complex ones |
| **Spend alerts** | Automated alerts at 50%, 75%, 90% of monthly budget |
| **Kill switch** | Ability to disable AI features without deploying code (feature flag) |

### Testing Non-Deterministic Outputs

Traditional unit tests (input → expected output) don't work for LLM responses. Instead:

- **Evaluation suites**: Define rubrics (relevance, safety, format compliance, factual accuracy) and score outputs against them
- **Golden dataset testing**: Maintain a set of inputs with acceptable output ranges; flag regressions
- **Boundary testing**: Test with adversarial inputs (prompt injection attempts, edge cases, empty inputs, extremely long inputs)
- **A/B testing**: For prompt changes, compare metrics (user satisfaction, task completion, cost) between variants
- **Human review**: Periodic manual review of AI outputs in production; sample-based quality auditing

---

## Cross-Cutting Concern: Design System Architecture *(Expanded in v2)*

*The original document covered token hierarchy as checkboxes. This section provides the framework for building and governing a complete design system.*

### Token Architecture (Three Layers)

```
Layer 1: Primitives        Layer 2: Semantic Tokens       Layer 3: Component Tokens
─────────────────          ────────────────────────       ──────────────────────────
green.600: #059669    →    brand: green.600           →   button.primary.bg: brand
warmGray.900: #1C1917 →    textPrimary: warmGray.900  →   card.title.color: textPrimary
4px base unit         →    spacing.sm: 8px            →   input.padding: spacing.sm
```

**Rules:**
- Components NEVER reference primitives directly
- Switching themes = remapping primitives → semantic layer; zero component changes
- Semantic tokens describe *role* ("brand", "danger", "surface"), never *appearance* ("green", "light")

### Component API Standards

Consistent component APIs reduce cognitive load and make the system predictable:

| Convention | Pattern | Example |
|-----------|---------|---------|
| **Variants** | `variant` prop for visual style | `<Button variant="primary \| secondary \| ghost">` |
| **Sizes** | `size` prop with t-shirt scale | `<Input size="sm \| md \| lg">` |
| **States** | Boolean props for states | `<Button disabled loading>` |
| **Composition** | Compound components for complex UI | `<Card><Card.Header /><Card.Body /></Card>` |
| **Polymorphism** | `as` prop for element/component swapping | `<Button as="a" href="/link">` |
| **Escape hatch** | `className` always accepted | `<Card className="custom-override">` |
| **Ref forwarding** | All interactive components forward refs | `const Button = forwardRef(...)` |

### Motion Design Principles

| Category | Duration | Easing | Examples |
|----------|----------|--------|----------|
| **Micro-interactions** | 100–150ms | `ease-out` | Button press, toggle, checkbox |
| **Transitions** | 200–300ms | `ease-in-out` | Dropdown open, tab switch, fade |
| **Page-level** | 300–500ms | `ease-out` | Modal entrance, page transition |
| **Data visualization** | 500–800ms | `ease-in-out` | Chart animation, progress fill |

**Non-negotiable:** All motion respects `prefers-reduced-motion`. When reduced motion is preferred, replace animations with instant state changes or simple opacity fades.

### Content Design (Microcopy) Standards

Consistent microcopy is as important as consistent visual design:

| Pattern | Guideline | Good Example | Bad Example |
|---------|-----------|-------------|------------|
| **Error messages** | Say what happened + what to do next | "That email is already registered. Try signing in instead." | "Error 409" |
| **Empty states** | Explain what will appear + provide action | "No allocations yet. Start by submitting your preferences." | "No data" |
| **Confirmation dialogs** | Verb-noun button labels, not Yes/No | "Delete 3 items" / "Keep items" | "Yes" / "No" |
| **Loading states** | Describe what's happening if >2s | "Running allocation algorithm…" | (spinner only) |
| **Success messages** | Confirm what happened, suggest next step | "Preferences saved. You'll be notified when allocation runs." | "Success!" |
| **Placeholder text** | Show format, not instructions | "jane@example.com" | "Enter your email here" |

### Responsive Design Strategy

**Breakpoint system (recommended):**

| Token | Value | Target |
|-------|-------|--------|
| `screen.sm` | 640px | Large phones (landscape) |
| `screen.md` | 768px | Tablets |
| `screen.lg` | 1024px | Small laptops |
| `screen.xl` | 1280px | Desktops |
| `screen.2xl` | 1536px | Large screens |

**Principles:**
- Mobile-first CSS: base styles for mobile, `@media (min-width)` to enhance
- Fluid typography: `font-size: clamp(1rem, 0.5rem + 1vw, 1.25rem)` — no jarring jumps
- Container queries: use for component-level responsiveness (card that works in sidebar AND main content)
- Test at breakpoints AND between them — responsive isn't just "looks good at 768px and 1024px"

---

## Cross-Cutting Concern: Dependency Management *(New in v2)*

*Dependencies are a cross-cutting concern from Tier 1 onward. Every `npm install` is a decision with security, maintenance, and legal implications.*

### Evaluation Criteria for Adding a Dependency

Before adding a package, evaluate against these criteria:

| Criterion | Question | Red Flag |
|----------|----------|----------|
| **Necessity** | Can this be done in <50 lines of code without the package? | Adding a package for trivial functionality (e.g., `is-odd`) |
| **Maintenance** | When was the last commit? Are issues being triaged? | No commits in 12+ months; growing issue backlog |
| **Bundle impact** | What's the gzipped size? Does it tree-shake? | >50KB for a utility; no ESM build |
| **Security** | Any known vulnerabilities? How quickly are they patched? | Open CVEs; slow security response |
| **License** | Is the license compatible with your project? | GPL in a proprietary project; no license specified |
| **Alternatives** | Is there a smaller/better-maintained alternative? | Using a 200KB library when a 5KB one does the same thing |
| **Transitive dependencies** | How deep is the dependency tree? | Package with 50+ transitive dependencies for simple functionality |

### License Compatibility Quick Reference

| License | Can Use in Proprietary Code? | Must Open-Source Your Code? | Notes |
|---------|:---------------------------:|:--------------------------:|-------|
| MIT | Yes | No | Most permissive; preferred |
| Apache 2.0 | Yes | No | Patent grant included |
| BSD (2/3-clause) | Yes | No | Similar to MIT |
| ISC | Yes | No | Simplified MIT |
| MPL 2.0 | Yes | No (only modified files) | File-level copyleft |
| LGPL | Yes (with care) | No (if dynamically linked) | Consult legal for static linking |
| GPL | Depends | Yes (if distributed) | Viral; consult legal |
| AGPL | Depends | Yes (even for SaaS) | Most restrictive; consult legal |
| Unlicensed / No license | **No** | N/A | No license ≠ public domain; avoid |

### Lock File Management

- **Always commit lock files** (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`) to version control
- **Never edit lock files manually** — let the package manager handle it
- **Use exact versions in lock files** — lock files exist to make builds reproducible
- **Audit regularly** — run `npm audit` or equivalent in CI; block deploys on critical vulnerabilities

### When to Vendor vs. Install

| Approach | When to Use |
|----------|------------|
| **Install (npm/yarn/pnpm)** | Default for most packages; automatic updates via dependabot |
| **Vendor (copy into repo)** | Package is unmaintained but you need it; you need to patch it; critical dependency you want to control completely |
| **Fork** | You need ongoing changes the maintainer won't accept; the package is abandoned but complex |
| **Contribute upstream** | Your fix is generally useful; preferred over forking when maintainer is responsive |

---

## Cross-Cutting Concern: Technology Selection Principles *(New in v2)*

*This document tells you what to do but not how to choose tools. This section provides a framework for technology decisions.*

### Decision Framework

When selecting a technology (framework, database, hosting, etc.), evaluate across these dimensions:

| Dimension | Questions to Ask |
|-----------|-----------------|
| **Team fit** | Does the team already know this? What's the learning curve? Can we hire for it? |
| **Community & ecosystem** | Is there an active community? Are there enough libraries/plugins? Stack Overflow answers? |
| **Maturity & stability** | How many major versions? Breaking changes frequency? Who backs it? |
| **Performance ceiling** | Will it handle our scale? What are the known bottlenecks? |
| **Escape hatches** | If this doesn't work out, how painful is migration? Are we locked in? |
| **Operational cost** | Hosting costs at our scale? DevOps complexity? Monitoring tooling available? |
| **Alignment with architecture** | Does it support our rendering strategy, deployment model, and data patterns? |

### Anti-Patterns in Technology Selection

| Anti-Pattern | Why It's Dangerous |
|-------------|-------------------|
| **Résumé-driven development** | Choosing tech because it's trendy, not because it fits the problem |
| **Premature optimization** | Choosing Kubernetes for a 3-person team's first product |
| **Ignoring operational cost** | Choosing a "free" framework that requires expensive infrastructure |
| **Framework maximalism** | Using a full framework (Next.js, Rails) for a static marketing page |
| **The "we might need it" trap** | Adding complexity for hypothetical future requirements |
| **Ignoring the exit** | Choosing a vendor with no data export or migration path |

### Document Your Decisions

For any significant technology choice, write a brief ADR (Architecture Decision Record):

```
## ADR-001: Use Next.js for Fair Time Off web application

**Status:** Accepted
**Date:** 2026-01-15
**Context:** We need SSR for SEO on marketing pages, CSR for the authenticated dashboard, 
and API routes for the allocation algorithm.
**Decision:** Next.js 15 with App Router
**Alternatives Considered:** Remix (less mature ecosystem), Vite + React Router (no SSR built-in), 
Astro (weak for complex interactive apps)
**Consequences:** Locked to Vercel ecosystem for best DX (but deployable elsewhere). 
Team already knows React. App Router has learning curve vs. Pages Router.
```

---

## Cross-Cutting Concern: Structural Integrity *(New in v2.02)*

*Every fix, feature, and integration must be implemented at the correct structural level. When a problem is solved at the wrong layer — or with a workaround instead of a proper fix — it creates fragility that compounds over time. This section codifies the standard: solve problems where they originate, not where they surface.*

### The Principle

**A solution must address the root cause at the correct structural level.** If a CLI command needs to run headlessly, add a `--non-interactive` flag to the CLI — don't parse stdout and fake keystrokes. If data needs to sync between two systems, build a sync endpoint — don't tell users to remember a manual step. If a UI component needs state from a database, query the database — don't cache a stale copy and hope it stays current.

### What Structural Integrity Means

| Correct | Incorrect |
|---------|-----------|
| Add a `--yes` flag to a script with interactive prompts | Detect prompt text in stdout and auto-send newlines |
| Create an API endpoint to sync state after actions | Require users to manually run `hub push` after every step |
| Read filesystem state as source of truth | Trust a database field that was populated months ago |
| Modify the source that generates the problem | Add a wrapper that suppresses the symptom |
| Fix the data model to support the use case | Add a special-case `if` branch in every consumer |
| Fail visibly with an actionable error message | Silently return empty data and let downstream code crash |

### Anti-Patterns

| Anti-Pattern | Why It Fails | Structural Fix |
|-------------|-------------|----------------|
| **Stdout scraping** | Breaks when text changes; invisible to maintainers | Add a flag, return structured output, or use an API |
| **Manual sync steps** | Humans forget; state drifts silently | Auto-sync after the action that changes state |
| **Swallowed errors** | `catch {}` with no rethrow hides the real problem | Throw with context, or handle explicitly with user-facing feedback |
| **Workaround-first** | "Quick fix now, proper fix later" — later never comes | Implement the structural fix immediately; the first implementation should be the right one |
| **Copy-paste adaptation** | Duplicates code instead of extending the source | Modify the original to support the new use case (add parameters, configuration, etc.) |
| **UI-only fixes** | Hiding a broken state behind CSS or conditional rendering | Fix the data/logic that produces the broken state |

### Decision Test

Before implementing any fix or feature, ask:

1. **Where does this problem originate?** (Not where it surfaces — where it *starts*.)
2. **Am I solving it at that level, or somewhere downstream?**
3. **If the upstream system changes, does my fix still work?** If not, it's a workaround.
4. **Would a new developer understand why this code exists?** If it requires a comment explaining the hack, it needs a structural fix.
5. **Does this create a manual step someone has to remember?** If yes, automate it or eliminate the need.

### Structural Integrity by Tier

**All Tiers:**
- [ ] **No swallowed errors** — every `catch` block either handles the error meaningfully or rethrows with context
- [ ] **No manual sync steps** — if an action changes state in one system, it propagates automatically
- [ ] **Actionable error messages** — errors tell the user (or developer) what went wrong and what to do about it

**Tier 2+:**
- [ ] **CLI commands support headless execution** — any command that might be called by automation supports `--yes` or `--non-interactive`
- [ ] **Source of truth is singular** — for any piece of state, exactly one system is authoritative; others read from it

**Tier 3+:**
- [ ] **No workaround comments** — code should not contain comments like "hack", "workaround", "TODO: fix properly later"
- [ ] **API contracts are explicit** — no relying on response format assumptions; validate and handle errors from every external call

**Tier 4:**
- [ ] **Filesystem and database are reconcilable** — if both store state, there is an automated mechanism to detect and resolve drift
- [ ] **Interactive gates have non-interactive equivalents** — governance checks (approval flows, gate reviews) can be satisfied programmatically for automation pipelines

---

## Data Privacy & Protection Principles

*Privacy isn't a tier—it's a cross-cutting concern that scales with what data you collect. A Tier 1 site with a newsletter signup has privacy obligations. Apply the relevant principles based on your data practices, not just your tier.*

### Foundational Privacy Principles (GDPR-Derived, Globally Applicable)

These seven principles form the foundation of most modern privacy law. Even if you're not subject to GDPR, these are best practices:

| Principle | What It Means | Implementation |
|-----------|---------------|----------------|
| **Lawfulness, Fairness, Transparency** | Have a legal basis for processing; don't deceive users | Clear privacy policy; no dark patterns; legitimate purpose for each data point |
| **Purpose Limitation** | Collect data for specified, explicit purposes; don't repurpose without consent | Document why you collect each field; don't use email signups for unrelated marketing |
| **Data Minimization** | Collect only what's necessary | Don't require phone number for newsletter; question every form field |
| **Accuracy** | Keep data accurate and up to date | Allow users to update their info; don't retain stale data |
| **Storage Limitation** | Don't keep data longer than needed | Define retention periods; automated deletion; don't hoard "just in case" |
| **Integrity & Confidentiality** | Protect against unauthorized access, loss, destruction | Encryption, access controls, backups, security measures |
| **Accountability** | Demonstrate compliance; document your practices | Privacy impact assessments, processing records, audit trails |

### AI-Specific Privacy Considerations *(New in v2)*

When using AI/LLM services, additional privacy concerns arise:

| Concern | Requirement |
|---------|------------|
| **Data sent to AI providers** | Document exactly what user data is sent to third-party AI APIs; include in privacy policy; ensure DPAs are in place with providers |
| **Training data opt-out** | Verify your AI provider does not train on your users' data (or obtain consent if they do); use API-tier service agreements, not consumer-tier |
| **AI-generated personal data** | If AI infers information about users (preferences, classifications, risk scores), this is personal data subject to privacy law |
| **Right to explanation** | Where AI makes decisions affecting users, users may have the right to understand the logic; maintain audit trails of AI inputs/outputs |
| **Data minimization for AI** | Only send the minimum data necessary for the AI task; strip PII where possible before sending to providers |

### Privacy by Design Principles

Build privacy in from the start, not as an afterthought:

- [ ] **Proactive, Not Reactive** – Anticipate privacy issues before they occur
- [ ] **Privacy as Default** – No action required from user to protect their privacy (opt-in, not opt-out)
- [ ] **Privacy Embedded in Design** – Not a bolt-on; integral to system architecture
- [ ] **Full Functionality** – Avoid false tradeoffs (privacy vs. functionality)
- [ ] **End-to-End Security** – Protect data throughout its lifecycle
- [ ] **Visibility & Transparency** – Operations verifiable by users and regulators
- [ ] **Respect for User Privacy** – User-centric; empower individuals

### Regulatory Awareness by Region

| Regulation | Jurisdiction | Key Triggers | Notable Requirements |
|------------|--------------|--------------|----------------------|
| **GDPR** | EU/EEA (+ anyone processing EU residents' data) | Any personal data of EU residents | Consent, data subject rights, 72-hour breach notification, DPAs, cross-border transfer rules |
| **CCPA/CPRA** | California (+ often applied US-wide) | >$25M revenue, or >50K consumers' data, or >50% revenue from selling data | Right to know, delete, opt-out of sale; "Do Not Sell" link |
| **PIPEDA** | Canada | Commercial activity involving personal info | Consent, access rights, accountability |
| **LGPD** | Brazil | Processing data of Brazil residents | Similar to GDPR; legal basis required |
| **COPPA** | United States | Collecting data from children under 13 | Verifiable parental consent required |
| **HIPAA** | United States | Protected Health Information (PHI) | Business Associate Agreements, encryption, access controls, audit trails |
| **PCI-DSS** | Global (payment cards) | Storing, processing, or transmitting cardholder data | Specific technical controls, network segmentation, regular assessments |
| **EU AI Act** | EU/EEA | AI systems deployed in or affecting EU residents | Risk classification, transparency requirements, human oversight for high-risk AI *(New in v2)* |

*Note: You're likely subject to multiple regulations. When in doubt, apply the strictest standard.*

### Data Subject Rights (Build Capability For These)

Users have rights. Your system needs technical capability to fulfill them:

| Right | Description | Technical Implication |
|-------|-------------|----------------------|
| **Access** | User can request copy of their data | Export functionality; know where all user data lives |
| **Rectification** | User can correct inaccurate data | Edit functionality across all data stores |
| **Erasure ("Right to be Forgotten")** | User can request deletion | Delete cascades properly; backups addressed; anonymization option |
| **Portability** | User can get data in machine-readable format | JSON/CSV export; standard formats |
| **Restriction** | User can limit processing | Ability to "freeze" an account without deleting |
| **Objection** | User can object to certain processing | Opt-out mechanisms that actually stop processing |
| **No Automated Decision-Making** | Right to human review of significant automated decisions | Human override capability for algorithmic decisions; particularly relevant for AI-powered features *(Note added in v2)* |

### Cookie & Tracking Consent

| Cookie Category | Examples | Consent Required? |
|-----------------|----------|-------------------|
| **Strictly Necessary** | Session cookies, authentication, security | No (but disclose) |
| **Functional/Preferences** | Language, theme, saved settings | Yes (in EU/UK) |
| **Analytics** | Google Analytics, Plausible, Mixpanel | Yes (in EU/UK) |
| **Marketing/Advertising** | Ad trackers, retargeting pixels, social widgets | Yes (everywhere, practically) |

**Implementation Requirements:**
- [ ] **Consent Before Tracking** – No analytics/marketing cookies until explicit consent (EU)
- [ ] **Granular Consent** – Users can accept some categories, reject others
- [ ] **Easy Withdrawal** – As easy to withdraw consent as to give it
- [ ] **Consent Records** – Log when consent was given, what version of policy
- [ ] **Respect Browser Signals** – Honor Do Not Track / Global Privacy Control where required
- [ ] **No Cookie Walls** – Can't force consent to access content (EU guidance)

### Privacy Engineering Patterns

**Data Handling:**
- [ ] **Pseudonymization** – Replace identifiers with tokens; reversible with key
- [ ] **Anonymization** – Irreversibly remove identifying information (truly anonymous data isn't "personal data")
- [ ] **Data Masking** – Hide sensitive portions (show last 4 of SSN)
- [ ] **Tokenization** – Substitute sensitive data with non-sensitive tokens (especially for payments)
- [ ] **Encryption at Rest** – Stored data encrypted; key management strategy
- [ ] **Encryption in Transit** – TLS everywhere; no sensitive data in URLs
- [ ] **Field-Level Encryption** – Extra protection for highly sensitive fields

**Access & Retention:**
- [ ] **Access Logging** – Who accessed what personal data, when
- [ ] **Purpose-Based Access Control** – Access granted per purpose, not just role
- [ ] **Retention Schedules** – Defined per data category; automated enforcement
- [ ] **Secure Deletion** – Actually delete, not just mark inactive; address backups

**Third Parties:**
- [ ] **Data Processing Agreements (DPAs)** – Contracts with all processors (analytics, email, hosting, AI providers)
- [ ] **Subprocessor Inventory** – Know who your vendors share data with
- [ ] **Cross-Border Transfer Mechanisms** – SCCs, adequacy decisions, or other valid mechanisms for international transfers
- [ ] **Vendor Security Assessment** – Evaluate processors' security practices

### Privacy Documentation

| Document | Purpose | When Required |
|----------|---------|---------------|
| **Privacy Policy** | Public disclosure of data practices | Always (any data collection) |
| **Cookie Policy** | Specific disclosure of tracking | Any cookies beyond strictly necessary |
| **Data Processing Agreement** | Contract with vendors processing your data | GDPR: required with all processors |
| **Records of Processing Activities (RoPA)** | Internal documentation of all processing | GDPR: required for most controllers |
| **Privacy Impact Assessment (PIA/DPIA)** | Risk assessment for high-risk processing | GDPR: required for high-risk; good practice always |
| **Data Retention Schedule** | How long each data type is kept | Best practice; demonstrates storage limitation |
| **Breach Response Plan** | Procedure for handling data breaches | Best practice; GDPR requires 72-hour notification |
| **AI Data Processing Addendum** | Additional disclosure for AI-processed data | When user data flows to AI providers *(New in v2)* |

### Privacy Checklist by Tier

**Tier 1 (if collecting any data, even email):**
- [ ] Privacy policy (what you collect, why, who you share with)
- [ ] Cookie disclosure (even for analytics)
- [ ] Secure form submission (HTTPS)
- [ ] Data minimization (don't collect what you don't need)

**Tier 2 (forms, calculators with data):**
- [ ] All of Tier 1
- [ ] Cookie consent mechanism (if using analytics/marketing)
- [ ] Clear consent for marketing communications
- [ ] Data retention awareness (how long do you keep form submissions?)
- [ ] DPA with form processor if using third-party (Typeform, etc.)

**Tier 3 (authenticated apps):**
- [ ] All of Tier 1-2
- [ ] Data subject rights implementation (access, deletion, export)
- [ ] Consent management system
- [ ] DPAs with all processors (including AI providers)
- [ ] Access logging for personal data
- [ ] Retention schedules defined and enforced
- [ ] Privacy policy covers all processing activities (including AI)
- [ ] Breach notification procedure

**Tier 4 (platform/regulated):**
- [ ] All of Tier 1-3
- [ ] Records of Processing Activities (RoPA)
- [ ] Privacy Impact Assessments for new features (especially AI features)
- [ ] Cross-border transfer mechanisms documented
- [ ] Subprocessor management
- [ ] Regular privacy audits
- [ ] Data Protection Officer (if required)
- [ ] Privacy training for team
- [ ] Automated compliance tooling
- [ ] AI-specific compliance documentation (EU AI Act if applicable)

---

## Quick Reference: Principle Application Matrix

| Principle | T1 | T2 | T3 | T4 |
|-----------|:--:|:--:|:--:|:--:|
| HTTPS/TLS | ✓ | ✓ | ✓ | ✓ |
| Security Headers | ✓ | ✓ | ✓ | ✓ |
| Input Validation | ✓ | ✓ | ✓ | ✓ |
| Semantic Tokens | ✓ | ✓ | ✓ | ✓ |
| Core Web Vitals | ✓ | ✓ | ✓ | ✓ |
| WCAG Accessibility | ✓ | ✓ | ✓ | ✓ |
| Privacy Policy | ✓ | ✓ | ✓ | ✓ |
| Data Minimization | ✓ | ✓ | ✓ | ✓ |
| SEO Fundamentals | ✓ | ✓ | ✓ | ✓ |
| Automated Deployment | ✓ | ✓ | ✓ | ✓ |
| Dependency Evaluation | ✓ | ✓ | ✓ | ✓ |
| CSRF Protection | | ✓ | ✓ | ✓ |
| Rate Limiting | | ✓ | ✓ | ✓ |
| State Management | | ✓ | ✓ | ✓ |
| Cookie Consent | | ✓ | ✓ | ✓ |
| DPAs with Processors | | ✓ | ✓ | ✓ |
| Dependency Scanning in CI | | ✓ | ✓ | ✓ |
| Error/Edge Case UX | | ✓ | ✓ | ✓ |
| Visual Regression Testing | | ✓ | ✓ | ✓ |
| Content Design Standards | | ✓ | ✓ | ✓ |
| Client-Side Error Capture | | ✓ | ✓ | ✓ |
| RBAC/Authentication | | | ✓ | ✓ |
| SOLID Principles | | | ✓ | ✓ |
| 12-Factor App | | | ✓ | ✓ |
| Structured Logging | | | ✓ | ✓ |
| CI/CD Pipeline | | | ✓ | ✓ |
| Testing Pyramid | | | ✓ | ✓ |
| Data Subject Rights | | | ✓ | ✓ |
| Retention Schedules | | | ✓ | ✓ |
| Breach Response Plan | | | ✓ | ✓ |
| Frontend Architecture | | | ✓ | ✓ |
| AI Prompt Injection Defense | | | ✓ | ✓ |
| AI Output Guardrails | | | ✓ | ✓ |
| AI Evaluation Framework | | | ✓ | ✓ |
| Load & Performance Testing | | | ✓ | ✓ |
| Contract Testing | | | ✓ | ✓ |
| Zero Trust | | | | ✓ |
| Distributed Patterns | | | | ✓ |
| SLOs/Error Budgets | | | | ✓ |
| Chaos Engineering | | | | ✓ |
| Privacy Impact Assessments | | | | ✓ |
| Cross-Border Transfer Mechanisms | | | | ✓ |
| Records of Processing (RoPA) | | | | ✓ |
| Multi-Model AI Strategy | | | | ✓ |
| AI Observability | | | | ✓ |
| AI Regulatory Compliance | | | | ✓ |
| No Swallowed Errors | ✓ | ✓ | ✓ | ✓ |
| Actionable Error Messages | ✓ | ✓ | ✓ | ✓ |
| No Manual Sync Steps | ✓ | ✓ | ✓ | ✓ |
| Headless CLI Support | | ✓ | ✓ | ✓ |
| Singular Source of Truth | | ✓ | ✓ | ✓ |
| No Workaround Comments | | | ✓ | ✓ |
| Explicit API Contracts | | | ✓ | ✓ |
| FS/DB Drift Reconciliation | | | | ✓ |
| Non-Interactive Gate Equivalents | | | | ✓ |

---

## UX Behavior Settings

*Note: These settings involve tradeoffs between content protection and usability/accessibility. Evaluate carefully before disabling default browser behaviors.*

Determine content protection preferences. **Default is to leave all behaviors enabled** (true). Only disable when there's a specific business requirement, and understand the tradeoffs:

| Setting | Default | Accessibility Impact When Disabled |
|---------|:-------:|-----------------------------------|
| Text Selection | `true` | **High** — Disabling breaks screen readers, copy-paste workflows, text-to-speech, browser "find on page," and assistive technology. WCAG does not require text selection, but disabling it creates significant barriers. |
| Right-Click Context Menu | `true` | **Medium** — Disabling removes access to browser accessibility features (translate, read aloud, inspect element for debugging). Trivially bypassed by disabling JavaScript. |
| Image Dragging | `true` | **Low** — Minimal accessibility impact. Still trivially bypassed via dev tools or "Save Image As." |

**Recommendation:** Leave all three enabled unless you have a specific, documented business requirement. These are deterrents, not security measures — determined users can bypass all of them via browser dev tools. The accessibility cost is real; the protection is cosmetic.

If you do choose to disable any of these, implementation:

**Text Selection: `false`** → Add to `reset.css`:
```css
* {
  -webkit-user-select: none;
  user-select: none;
}
/* CRITICAL: Always preserve selection in input areas */
input, textarea, [contenteditable="true"] {
  -webkit-user-select: text;
  user-select: text;
}
```

**Right-Click Context Menu: `false`** → Add to app entry (`main.jsx`):
```javascript
document.addEventListener('contextmenu', (e) => e.preventDefault());
```

**Image Dragging: `false`** → Add to `reset.css`:
```css
img {
  -webkit-user-drag: none;
  user-drag: none;
}
```

---

## Usage Instructions

When starting a new project:

1. **Answer the 10 classification questions** honestly (9 original + AI/LLM)
2. **Check auto-escalation rules** — certain "yes" answers set a minimum tier
3. **Determine your tier** based on escalation rules + "yes" count
4. **Apply all principles from your tier and below** as your baseline checklist
5. **Apply relevant cross-cutting concerns** (Privacy, AI, Design System, Dependencies, Technology Selection) based on your project's characteristics, not just its tier
6. **Cherry-pick from higher tiers** if specific concerns warrant (e.g., Tier 2 site handling payments should pull in relevant Tier 3 security items)
7. **Document any intentional deviations** with rationale (use ADR format for significant decisions)
8. **Configure UX behavior settings** — evaluate content protection needs against accessibility tradeoffs

### Prompt Template for AI-Assisted Development

```
I'm building [PROJECT DESCRIPTION].

Classification answers:
- User Authentication: [yes/no]
- Data Persistence: [yes/no]
- Multi-Role Access: [yes/no]
- Third-Party Integrations: [yes/no]
- Real-Time Features: [yes/no]
- Transaction Sensitivity: [yes/no]
- Scale Expectations (>10k users): [yes/no]
- Team Size (multiple devs): [yes/no]
- Longevity (2+ years): [yes/no]
- AI/LLM Features: [yes/no]

UX Behavior Settings:
- Text Selection: [true/false]
- Right-Click Context Menu: [true/false]
- Image Dragging: [true/false]

Technology choices (if decided):
- Framework: [e.g., Next.js, Remix, Astro]
- Database: [e.g., PostgreSQL, Supabase, PlanetScale]
- Hosting: [e.g., Vercel, AWS, Railway]
- AI Provider: [e.g., Anthropic, OpenAI, none]

Based on this classification (including auto-escalation rules), apply the appropriate tier 
principles and cross-cutting concerns from the Universal Web Development Principles v2 
framework. Flag any areas where we should consider principles from a higher tier given the 
specific nature of this project.

[SPECIFIC REQUEST]
```

---

## Classification Decision Guide

Use this guide when you're unsure how to answer a classification question. Each entry explains what qualifies as "yes," gives concrete examples, and explains why it matters.

### 1. User Authentication

**What it means:** Users create accounts, log in, and have persistent identities across sessions.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Users create accounts with email/password | Visitors are anonymous |
| Social login (Google, Apple, etc.) | No login functionality |
| "Remember me" or persistent sessions | Password-protected pages use a single shared password |
| User-specific dashboards or profiles | Contact forms that collect email (but no accounts) |

**Why it matters:** Authentication introduces session management, password storage, account recovery flows, and becomes the foundation for authorization. It's a significant security surface area and architectural complexity jump.

**Gray area:** A site with a simple admin login (just you) to edit content is borderline—answer "yes" if the admin panel is custom-built, "no" if it's a managed CMS like WordPress where auth is handled for you.

---

### 2. Data Persistence

**What it means:** User-generated content is stored in a database and retrieved later. The application has "state" that survives server restarts.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Users submit data that's stored and displayed later | Forms just send emails (no storage) |
| Comments, posts, uploads, saved preferences | Calculators that compute and display (no saving) |
| Order history, transaction records | Static content managed in code/CMS |
| Any custom database (PostgreSQL, MongoDB, etc.) | Third-party forms (Typeform, Google Forms) |

**Why it matters:** Databases introduce backup requirements, migration strategies, query optimization, data integrity concerns, and significantly expand your security perimeter (SQL injection, data breaches).

**Gray area:** Using Airtable or Google Sheets as a lightweight backend counts as "yes"—you still own the data architecture concerns even if the database is managed.

---

### 3. Multi-Role Access

**What it means:** Different users have different permissions. Some users can do things others cannot.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Admin vs. regular user distinction | All logged-in users see/do the same things |
| Owner/Editor/Viewer permission levels | Single admin account (just you) |
| Manager can see team data, member sees own only | Public content, no restrictions by user |
| Approval workflows (submit → review → publish) | Binary: logged in or not logged in |

**Why it matters:** RBAC requires authorization checks throughout the application—not just "can they log in" but "can this user do this action to this resource." Permission bugs often create serious security vulnerabilities. You need to enforce at the API level, not just hide UI elements.

**Auto-escalation:** If "yes," minimum Tier 3.

**Gray area:** If you have "admin" and "user" roles but admin just means "can access /admin route," that's minimal RBAC—still answer "yes" but the complexity is lower than granular permissions.

---

### 4. Third-Party Integrations

**What it means:** Your application connects to external services via APIs—sending or receiving data programmatically.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Payment processing (Stripe, PayPal) | Embedding a YouTube video |
| CRM integration (Salesforce, HubSpot) | Adding Google Analytics (client-side snippet) |
| Email service (SendGrid, Mailchimp API) | Social share buttons |
| Calendar sync (Google Calendar API) | Linking to external sites |
| AI/ML APIs (OpenAI, Anthropic, etc.) | Using a hosted contact form widget |
| Pulling data from external sources | CDN-hosted libraries (jQuery, fonts) |

**Why it matters:** Third-party integrations introduce API key management, error handling for external failures, rate limits you don't control, data flowing outside your system, and versioning concerns when APIs change. Each integration is a potential failure point and security consideration.

**Gray area:** Webhooks (incoming) count as "yes." Zapier/Make connections might be "yes" if you're building the integration, "no" if it's entirely configured in their UI with no custom code.

---

### 5. Real-Time Features

**What it means:** Data updates on the user's screen without them refreshing the page, based on events happening elsewhere (other users, external systems, time-based).

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Live chat or messaging | Contact forms |
| Collaborative editing (Google Docs-style) | Single-user forms that save on submit |
| Live notifications/alerts | Email notifications |
| Real-time dashboards (stock tickers, live stats) | Dashboards that refresh on page load |
| Multiplayer features | Turn-based interactions |
| Presence indicators ("3 users viewing") | View counters that require refresh |
| Live comments/feeds | Comment sections that require refresh |

**Why it matters:** Real-time requires persistent connections (WebSockets, Server-Sent Events), pub/sub infrastructure, handling connection drops/reconnects, and careful state synchronization. It's a fundamentally different architecture than request/response. Scaling real-time is harder than scaling stateless HTTP.

**Auto-escalation:** If "yes," minimum Tier 3.

**Gray area:** Polling every 30 seconds for updates is not "real-time"—it's frequent refreshing. If updates need to appear within 1-2 seconds of the event, that's real-time. Auto-save that syncs in the background is borderline; answer "yes" if conflicts between users are possible.

---

### 6. Transaction Sensitivity

**What it means:** The application handles data where errors, breaches, or mishandling have serious consequences—financial, legal, health, or privacy implications.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Payment processing (any amount) | Displaying public information |
| Bank account or financial data | User preferences (theme, language) |
| Health/medical information (PHI) | Blog comments |
| Social Security Numbers, government IDs | Newsletter signups (email only) |
| Legal documents, contracts | General contact forms |
| Children's data (COPPA) | Non-sensitive user profiles |
| Precise location tracking | Approximate location (city-level) |
| Biometric data | Username and display name |

**Why it matters:** Sensitive data triggers compliance requirements (PCI-DSS, HIPAA, GDPR special categories), demands encryption at rest, requires audit trails, and significantly raises the stakes for security. A breach isn't just embarrassing—it's potentially illegal and harmful to users.

**Auto-escalation:** If "yes," minimum Tier 3.

**Gray area:** Email addresses alone are borderline—they're PII but low sensitivity. Answer "no" unless combined with other identifying data. When in doubt, ask: "If this data leaked, would users face financial harm, discrimination, or identity theft?" If yes, answer "yes."

---

### 7. Scale Expectations (>10,000 users)

**What it means:** The application will serve a large user base, either concurrently or in total active users.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Public SaaS product | Internal company tool (<500 employees) |
| Consumer app with growth ambitions | Client project for small business |
| Viral/marketing-driven traffic expected | Portfolio or personal site |
| API serving many clients | Local community tool |
| E-commerce with significant volume | Boutique/artisan shop |

**Why it matters:** Scale forces architectural decisions: caching strategies, database optimization, horizontal scaling, CDN configuration, rate limiting, and capacity planning. What works for 100 users may fall over at 10,000. Costs scale differently too—inefficient queries become expensive.

**Gray area:** "We hope it goes viral" isn't a "yes"—answer based on realistic 12-month projections. However, if you genuinely expect rapid growth, design for it. A calculator that might get shared widely on social media is worth a "yes" even if starting small.

---

### 8. Team Size (Multiple Developers)

**What it means:** More than one developer will write code in this codebase, now or in the foreseeable future.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| 2+ developers contributing code | Solo developer, staying solo |
| Plan to hire/contract developers | Personal project |
| Handoff to client's dev team expected | Client will only do content updates |
| Open source with contributors | Internal tool you'll always maintain alone |

**Why it matters:** Multiple developers require code standards, documentation, clear architecture, consistent patterns, meaningful commit messages, code review processes, and avoiding "it works on my machine" situations. Technical debt accumulates faster without these, and onboarding becomes painful.

**Gray area:** "I might hire someone someday" is a "no" for now—you can always add rigor later. But if you're building something you'll hand off in 6 months, answer "yes" and build for maintainability from the start.

---

### 9. Longevity (2+ Years Maintenance)

**What it means:** This codebase will be actively maintained, extended, or depended upon for two or more years.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| Core business application | Campaign landing page (3-month lifespan) |
| Product you're building a company around | Event website (one-time use) |
| Client retainer for ongoing development | Prototype/proof of concept |
| Platform others build on | Throwaway experiment |
| Regulatory requirements for data retention | Portfolio piece |

**Why it matters:** Long-lived codebases face dependency rot, framework upgrades, team turnover, and evolving requirements. Decisions made today become constraints or technical debt tomorrow. Documentation, ADRs, upgrade paths, and maintainable architecture pay dividends over time.

**Gray area:** "It's an MVP" is often a "no," but if the MVP succeeding means you'll keep building on it, answer "yes"—MVPs that become production systems with MVP architecture are painful. Be honest about whether you'll actually throw it away.

---

### 10. AI/LLM Features *(New in v2)*

**What it means:** The application uses AI language models for content generation, classification, recommendations, search, or other intelligent features.

| Answer "Yes" If... | Answer "No" If... |
|-------------------|-------------------|
| User-facing AI chat, generation, or suggestions | Using AI only during development (Copilot, Claude Code) |
| AI-powered search or recommendations | Static content that was once AI-generated but is now fixed |
| LLM API calls in your application code | Embedding a third-party chatbot widget with no customization |
| AI classification or decision-making | Rule-based automation (if/then logic, not ML) |
| Fine-tuned models or RAG pipelines | Using AI for one-time data processing during build |

**Why it matters:** AI introduces non-deterministic outputs (can't be unit tested traditionally), prompt injection as a security attack vector, cost that scales with usage in hard-to-predict ways, and privacy concerns when user data flows to third-party model providers. It also introduces UX challenges around trust, transparency, and managing user expectations.

**Auto-escalation:** If "yes" with user-facing AI features, minimum Tier 3.

**Gray area:** An embedded chatbot widget from a third party (Intercom, Drift) where you don't control the AI is borderline — answer "no" for classification but still document the widget's data practices in your privacy policy. If you're customizing the AI behavior (custom prompts, RAG, fine-tuning), answer "yes."

---

### Quick Classification Examples

| Project | Auth | Data | Roles | Integrations | Real-Time | Sensitive | Scale | Team | Longevity | AI/LLM | **Tier** |
|---------|:----:|:----:|:-----:|:------------:|:---------:|:---------:|:-----:|:----:|:---------:|:------:|:--------:|
| Insurance advisor calculator | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ? | ✗ | ✓ | ✗ | **T1-T2** |
| Mortgage tool + saved scenarios | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ | ✗ | **T2** |
| Client portal for RIA | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ | ✓ | ✗ | **T3** (auto: Sensitive+Roles) |
| Fair Time Off (physician scheduling) | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ | ✓ | ✓ | ✓ | **T3-T4** (auto: Sensitive+Roles+AI) |
| Multi-tenant SaaS platform | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | **T4** |
| Marketing site for Barastone | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ | ✗ | **T1** |
| GetDigital client site (typical) | ✗ | ✓ | ✗ | ✓ | ✗ | ✗ | ✗ | ✗ | ✓ | ✗ | **T2** |

---

## Postel's Law Reminder

> "Be conservative in what you send, liberal in what you accept."

This applies at every tier—validate inputs gracefully, output strictly to spec. In the age of AI, this extends to: be conservative in what you send to LLM providers (minimize data), liberal in validating what you receive back (expect unexpected outputs).

---

*v2.01.02 | Designed for calibrated application across project complexity levels*
*Review cadence: Quarterly | Owner: @getdigital2020 | Next review: 2026-06-20*
