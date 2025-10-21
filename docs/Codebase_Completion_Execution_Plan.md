# Model Portfolio — Codebase Completion Execution Plan

> Purpose: A meticulous, phase-by-phase execution plan an AI coding agent (e.g., Claude Code, Codex, or this assistant) can follow to complete the repository into a production-ready **Model Portfolio Landing Page** that satisfies the PRD. Each phase is independently executable, contains a clear scope, an itemized file-level checklist, implementation notes, validation steps, and success criteria.

---

## Phase 0 — Preparation & Repo Hygiene (always run first)
**Goal:** Ensure the repository is in a deterministic, testable state and developer environment is standardized.

### What to do
- Inspect repo structure; create missing `.env.example`.
- Ensure `package.json` scripts exist for: `dev`, `build`, `start`, `lint`, `typecheck`, `test`, `playwright`.
- Add or validate `.editorconfig`, `.prettierignore`, `.gitignore`.
- Ensure `postinstall` `prisma generate` is harmless in CI.
- Create a `docs/` folder with `README.dev-setup.md` and `CONTRIBUTING.md`.

### Files to create/update
- `.env.example`
- `README.dev-setup.md`
- `CONTRIBUTING.md`
- `.github/workflows/ci.yml` (skeleton placeholder)
- `.editorconfig` (if missing)
- `.prettierignore` (if missing)

### Checklist
- [ ] Repo builds locally (`npm ci && npm run build`) without secrets.
- [ ] `npm run lint` passes or shows actionable lint failures.
- [ ] `npm run typecheck` completes.
- [ ] `prisma generate` runs without DB access (client generation only).
- [ ] `README.dev-setup.md` contains steps to run DB via Docker (postgres), set envs, and run dev.

### Validation / Commands
```bash
npm ci
npm run lint
npm run typecheck
npm run build
````

### Success criteria

* Developer onboarding steps are complete and reproducible.
* CI skeleton exists and will be filled in Phase 5.

---

## Phase 1 — Core Data Models & API (DB + tRPC)

**Goal:** Implement Prisma models, tRPC routers, server context, and the minimal API surface for Campaigns, Testimonials, and ContactSubmission.

### Rationale

Back-end models + APIs are foundational; front-end components will consume these endpoints. Keep APIs minimal and well-typed.

### Files to create/modify

* `prisma/schema.prisma` — add models (Campaign, Testimonial, ContactSubmission, optionally User for auth)
* `prisma/.env.example` — DB URL placeholder
* `src/server/db/client.ts` — Prisma client instantiation (singleton)
* `src/server/context.ts` — tRPC context creator (session, prisma)
* `src/server/trpc/router/index.ts` — root router
* `src/server/trpc/router/campaign.ts` — CRUD read endpoints (list, getBySlug)
* `src/server/trpc/router/testimonial.ts` — list endpoints
* `src/server/trpc/router/contact.ts` — create contact submission
* `src/server/trpc/schema.ts` or type helpers if needed
* `src/pages/api/trpc/[trpc].ts` or `src/app/api/trpc/route.ts` depending on app-router setup

### Checklist

* [ ] Prisma schema matches PRD models.
* [ ] `prisma generate` succeeds.
* [ ] tRPC config (transformer, error formatter) present and typed.
* [ ] `GET /api/trpc` basic health/type-check returns 200 in dev.
* [ ] Contact endpoint stores validated submissions in DB.

### Implementation notes

* Use Zod for input validation in tRPC procedures.
* For `Campaign.tags` use `String[]` type in Prisma and cast arrays in input.
* Protect endpoints that will be admin-only in future by TODO comment & middleware.

### Validation / Commands

* `npx prisma migrate dev --name init` (if real DB available) OR `npx prisma db push` for dev-only.
* Unit test `createContact` using in-memory test DB or sqlite.

### Success criteria

* API endpoints are fully typed, validated, and stored in the database (or schema can be pushed).
* tRPC client types can be imported on the front-end.

---

## Phase 2 — Authentication & Admin Skeleton

**Goal:** Add auth (NextAuth or @auth/next) configuration and a protected admin route skeleton (for future content editing).

### Files to create/modify

* `src/auth/[...nextauth].ts` or `src/app/api/auth/[...route]` depending on router
* `src/server/auth.ts` — auth helpers (getSessionServer, getCurrentUser types)
* `src/pages/admin/index.tsx` or `src/app/(admin)/page.tsx` — admin placeholder UI
* `src/components/AuthGuard.tsx` — HOC or wrapper for protecting pages
* `prisma` model `User` (if using DB adapter)
* `src/lib/oauth-readme.md` — instructions to configure OAuth providers

### Checklist

* [ ] `NEXTAUTH_URL` and `AUTH_SECRET` referenced in `.env.example`.
* [ ] Auth adapter configured for Prisma (optional in early stage).
* [ ] Admin route redirects to sign-in if not authenticated.
* [ ] Server-side helper to create/update test admin is documented.

### Validation

* Manual sign-in via configured provider (or credentials) works in dev.
* `getSession` server helper returns expected user info.

### Success criteria

* Basic auth integration exists and admin route is guarded.

---

## Phase 3 — UI: Layout, Core Pages & Components (mobile-first)

**Goal:** Implement core pages and shared UI components according to the UI/UX spec. Focus on accessibility, responsive layout, and component reusability.

### Files to create/modify (grouped by component/page)

**Global**

* `src/app/layout.tsx` (NextJS App Router) or `src/pages/_app.tsx` — root layout, fonts, global CSS & Tailwind variables
* `src/styles/globals.css` — Tailwind base + CSS variables
* `tailwind.config.js` — theme variables & color tokens

**Header & Nav**

* `src/components/Header/Header.tsx`
* `src/components/Header/NavLink.tsx`
* `src/components/Header/Hamburger.tsx`
* `src/components/Header/SocialIcons.tsx`

**Hero**

* `src/components/Hero/Hero.tsx`

**Featured Campaigns**

* `src/components/Campaigns/CampaignGrid.tsx`
* `src/components/Campaigns/CampaignCard.tsx`
* `src/app/portfolio/page.tsx` (or `src/pages/portfolio.tsx`)

**Campaign Detail**

* `src/app/portfolio/[slug]/page.tsx` — campaign details + gallery component
* `src/components/Gallery/Gallery.tsx` — responsive gallery with lightbox

**Testimonials & Recognition**

* `src/components/Testimonials/Carousel.tsx`
* `src/components/FeaturedIn/BrandLogos.tsx`

**Contact**

* `src/components/Contact/ContactForm.tsx`
* `src/app/contact/page.tsx` — page wrapper with contact meta

**Footer**

* `src/components/Footer/Footer.tsx`

**Shared / UI primitives**

* `src/components/ui/Button.tsx`
* `src/components/ui/Input.tsx`
* `src/components/ui/Select.tsx`
* `src/components/ui/Textarea.tsx`
* `src/components/ui/Card.tsx`
* `src/components/ui/Container.tsx`

### Implementation notes

* Use Tailwind utility classes; centralize tokens in `tailwind.config.js` and CSS variables.
* Use `next/image` for images with `priority` on hero, `loading="lazy"` on other images.
* For animations, use Framer Motion on key components (Hero fade-in, Card hover).
* All interactive elements must have accessible labels and keyboard support.
* Implement responsive grid: `grid-cols-1 sm:grid-cols-2 lg:grid-cols-3`.

### Checklist

* [ ] Header: desktop + mobile hamburger works; nav links route correctly.
* [ ] Hero: responsive, accessible alt text, CTA buttons link to portfolio and contact.
* [ ] Campaign grid: consumes tRPC `listCampaigns` and displays cards.
* [ ] Campaign detail: slug route reads data from tRPC `getCampaignBySlug`.
* [ ] Testimonials: carousel accessible, swipable on touch devices.
* [ ] Contact form: client-side validation integrated with server contact endpoint.
* [ ] Footer meets spec.
* [ ] All images have `alt` attributes and are optimized.

### Validation

* Run `npm run dev` and manually test on mobile emulator widths (375px, 768px, 1024px).
* Use Lighthouse (local) to check performance & accessibility spots.

### Success criteria

* Visual fidelity to the PRD components; pages are responsive and accessible.
* Components are modular, testable, and used consistently.

---

## Phase 4 — Forms, Validation, UX Polishing & Accessibility

**Goal:** Harden forms, add live validation, error states, ARIA attributes, keyboard interactions, and micro-interactions.

### Files to create/modify

* `src/components/Contact/ContactForm.tsx` — add client-side validation (Zod + react-hook-form)
* `src/lib/validators/contact.ts` — zod schema for contact form
* `src/components/Testimonials/Carousel.a11y.tsx` — accessibility wrapper
* `src/components/SkipToContent.tsx`
* `src/components/FocusRing.css` — focus states accessible
* `src/components/Loading/Spinner.tsx` — accessible spinner component

### Checklist

* [ ] Contact form disabled until required fields are valid.
* [ ] Error messages are shown inline under inputs with `aria-invalid`.
* [ ] All form fields have `label` elements connected via `htmlFor`.
* [ ] Skip-to-content link present and visible when focused.
* [ ] Color contrast passes WCAG AA (test key text on hero and CTA colors).
* [ ] Keyboard order is logical; carousel is operable with arrow keys and has aria-live for announcements.

### Validation

* Automated axe checks (Jest + `axe-core/react` or Playwright accessibility) pass.
* Manual keyboard-only navigation passes for all pages.

### Success criteria

* Forms robustly validate and are accessible.
* Accessibility test results documented in `ACCESSIBILITY_REPORT.md`.

---

## Phase 5 — Tests, CI/CD, and Observability

**Goal:** Add automated tests, CI pipeline, and runtime monitoring hooks.

### Files to create/modify

**Tests**

* `tests/unit/*` — Jest unit tests for components (Button, Input, validators)
* `tests/api/*` — integration tests for tRPC endpoints using a test DB or mocking
* `playwright.config.ts` — update base URL, add test suite
* `tests/e2e/home.spec.ts` — E2E for primary flow: open home, view portfolio, submit contact (mock email)

**CI**

* `.github/workflows/ci.yml` — installs deps, runs lint, typecheck, build, tests, Playwright (headless), and Prisma generate
* `.github/workflows/deploy.yml` — placeholder for deployment to Vercel (if used)

**Observability**

* `src/lib/sentry.ts` — Sentry initialization option (env gating)
* `src/lib/telemetry.md` — guidance on enabling

### Checklist

* [ ] Unit tests cover core utilities & components.
* [ ] Playwright E2E tests cover `Home → Portfolio → Campaign → Contact`.
* [ ] CI runs `npm ci`, `npm run lint`, `npm run typecheck`, `npm run build`, `npx playwright test`.
* [ ] Secrets are set in CI repository settings (no plaintext secrets in code).

### Validation / Commands

```bash
# Locally:
npm run test     # jest unit tests
npx playwright test

# In CI:
# Steps reflected in .github/workflows/ci.yml
```

### Success criteria

* CI green for master/main branch.
* E2E tests reliably pass in CI environment (use a test DB / seed data).

---

## Phase 6 — SEO, Performance Tuning & Launch Checklist

**Goal:** Final performance optimizations, SEO markup, metadata, and launch readiness.

### Files to create/modify

* `src/app/head.tsx` or `src/pages/_document.tsx` — SEO defaults, Open Graph, Twitter card
* `src/lib/seo.ts` — helper for dynamic metadata
* `next-sitemap.js` or `src/utils/sitemap.ts` — sitemap generation
* `public/robots.txt`
* `src/lib/image-optimizations.md` — rules for image sizes and formats (AVIF/WEBP)

### Checklist

* [ ] All pages have descriptive meta titles and descriptions.
* [ ] Schema.org markup for `Person` and `Organization` is present.
* [ ] Sitemap generated during build.
* [ ] Images are served with modern formats and responsive sizes.
* [ ] Lighthouse: performance ≥ 90, accessibility ≥ 90, SEO ≥ 90 (aim).
* [ ] Rollback plan documented.

### Validation

* Run `next build` and `next export` tests (if static parts exist).
* Run Lighthouse audit locally and document improvements.

### Success criteria

* Pass final QA checklist for accessibility, performance, and SEO.
* Deployment workflow documented and dry-run performed on staging.

---

## Cross-Phase Non-Functional Workstreams (run alongside phases)

These tasks span multiple phases and must be implemented incrementally.

### 1. Security & Secrets

* Add `SECURITY.md` and secrets handling guide.
* Ensure `AUTH_SECRET`, OAuth client secrets, and DB URLs are never committed.

### 2. Monitoring & Error reporting

* Add optional Sentry integration and server-side error reporting toggled by `SENTRY_DSN`.

### 3. Internationalization & Future Proofing

* Add i18n-ready patterns (use next-intl or built-in next i18n routing) — initial English only.

### 4. Component Library & Design Tokens

* Create `src/components/ui/tokens.css` or a `tokens.ts` exporting color & spacing tokens.
* Storybook (optional) or a UI showcase page to preview components.

### 5. Documentation

* Update `README.md` with deployment instructions, feature toggles, and runbook.
* Keep `API_REFERENCE.md` and `STYLE_GUIDE.md` updated per changes.

---

## File-by-file Reference (concise master list)

> This list is intended for the AI agent to create/complete files in the repo. Group and prioritize by phases.

**Phase 0**

* `.env.example`
* `README.dev-setup.md`
* `CONTRIBUTING.md`
* `.editorconfig`, `.prettierignore`
* `.github/workflows/ci.yml` (skeleton)

**Phase 1**

* `prisma/schema.prisma`
* `src/server/db/client.ts`
* `src/server/context.ts`
* `src/server/trpc/router/index.ts`
* `src/server/trpc/router/campaign.ts`
* `src/server/trpc/router/testimonial.ts`
* `src/server/trpc/router/contact.ts`
* `src/app/api/trpc/route.ts` or `src/pages/api/trpc/[trpc].ts`

**Phase 2**

* `src/app/api/auth/[...nextauth]/route.ts` or equivalent
* `prisma` `User` model (optional)
* `src/components/AuthGuard.tsx`
* `src/app/admin/page.tsx`

**Phase 3**

* `src/app/layout.tsx`
* `src/styles/globals.css`
* `tailwind.config.js`
* `src/components/Header/*`
* `src/components/Hero/Hero.tsx`
* `src/components/Campaigns/*`
* `src/app/portfolio/page.tsx`
* `src/app/portfolio/[slug]/page.tsx`
* `src/components/Gallery/Gallery.tsx`
* `src/components/Testimonials/*`
* `src/components/Contact/ContactForm.tsx`
* `src/components/Footer/Footer.tsx`
* `src/components/ui/*`

**Phase 4**

* `src/lib/validators/*`
* `src/components/SkipToContent.tsx`
* `src/components/FocusRing.css`

**Phase 5**

* `tests/*`
* `playwright.config.ts`
* `.github/workflows/ci.yml` (full)
* `src/lib/sentry.ts`

**Phase 6**

* `src/app/head.tsx`
* `src/lib/seo.ts`
* `next-sitemap.js`
* `public/robots.txt`

---

## Acceptance Criteria & Definition of Done (DOD)

For each phase, the AI agent should only mark the phase complete when **all** of the following are true:

1. **Buildable** — `npm run build` completes successfully.
2. **Lint & Typecheck** — `npm run lint` and `npm run typecheck` pass or failures are only in non-blocking rules that are documented.
3. **Tests** — Unit tests for the phase's components pass; critical E2E flows pass in local Playwright run (or are mocked).
4. **Accessibility** — Key pages pass basic axe or Lighthouse accessibility checks (no critical violations).
5. **Documentation** — README or phase notes updated describing what was implemented and how to run features.
6. **Security** — Secrets are not committed; env examples provided.

---

## Suggested AI Agent Workflow / Execution Pattern

Use an iterative create-test-commit cycle for each file or group of changes:

1. **Plan** — Identify target files and API contracts for the task.
2. **Generate** — Produce the source code for the file(s).
3. **Unit-test** — Add minimal tests for logic-heavy files (validators, API handlers).
4. **Run build/lint/typecheck** — Fix issues immediately.
5. **Integration test** — Manual dev run or Playwright snippet for UI flows.
6. **Commit with descriptive message** — Link to PRD section or task ID.
7. **Document** — Add small doc block in `docs/` mentioning design decisions and TODOs.

---

## Useful Implementation Patterns & Code Snippets (templates)

> These are small patterns the AI agent should reuse. (Not exhaustive; adapt to project structure.)

**tRPC procedure with Zod**

```ts
export const campaignRouter = createTRPCRouter({
  list: publicProcedure
    .input(z.object({ tag: z.string().optional(), take: z.number().optional() }))
    .query(async ({ ctx, input }) => {
      return ctx.prisma.campaign.findMany({ where: input.tag ? { tags: { has: input.tag } } : {}, take: input.take ?? 12 });
    }),
  getBySlug: publicProcedure.input(z.object({ slug: z.string() })).query(async ({ ctx, input }) => {
    return ctx.prisma.campaign.findUnique({ where: { slug: input.slug } });
  }),
});
```

**React accessible button**

```tsx
export function Button({ children, ...props }: ButtonProps) {
  return (
    <button
      {...props}
      className="rounded-2xl px-6 py-3 shadow-sm focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2"
    >
      {children}
    </button>
  );
}
```

**Contact form Zod schema**

```ts
export const contactSchema = z.object({
  firstName: z.string().min(1, "First name required"),
  lastName: z.string().optional(),
  email: z.string().email("Invalid email"),
  company: z.string().optional(),
  projectType: z.enum(["Editorial", "Runway", "Commercial", "Beauty", "Ambassador", "Other"]),
  message: z.string().min(10, "Message too short"),
});
```

---

## Final Remarks for the Agent

* **Be explicit**: create files with clear TODO markers for environment-specific secrets and admin-only access.
* **Small commits**: each component or router should be committed with a focused commit message referencing the PRD section.
* **Automate checks**: make CI stricter over time (start with basic jobs; add caching & parallelization later).
* **Document decisions**: when deviating from the PRD for technical reasons, document in `docs/` with rationale and alternative options.

---

## Immediate next step (for the AI agent to run now)

* Execute Phase 0 and Phase 1 in sequence: create `prisma/schema.prisma`, `src/server/*` tRPC router skeletons, `README.dev-setup.md`, and `.env.example`, then run `prisma generate` and `npm run typecheck` and fix blockers.

> If you'd like, I can now **generate all the files for Phase 1** (Prisma schema + tRPC router stubs + Prisma client) and show them here in the chat (or place them into the project canvas for iterative editing). Tell me to proceed and I will create the code artifacts for Phase 1 immediately.

---

https://chatgpt.com/share/68f76b32-1460-800c-a4e8-c3595195b988
