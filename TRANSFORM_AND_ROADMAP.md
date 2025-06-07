# LlamaCoder SaaS Deployment Pipeline


*** work with Cursor to understand how to have a multi-tenant (?) Vercel app that can receive these individual apps on a common Next substrate of primitives ***

*Author in Vite/Sandpack → Transform → Deploy as Next.js 15*

---

## 0. TL;DR
1. **Lightning-fast in-browser editing** (Vite + Sandpack) remains untouched.
2. **One-click transform** creates a fully-featured Next.js 15 project with SSR, API routes, Images, Edge Functions.
3. Deployment targets: Vercel (default), Netlify, or self-host tarball.
4. Twelve roadmap items (below) extend the pipeline into a full-blown SaaS platform.

---

## 1. Core Goals
| # | Goal |
|---|------|
| 1 | Preserve sub-second rebuilds during authoring. |
| 2 | Deliver production-grade Next.js SaaS out of the box. |
| 3 | Zero required edits to the user’s builder code. |
| 4 | Hide the transform complexity behind a **Deploy** button. |

---

## 2. Builder Layer
| Topic | Spec |
|-------|------|
| **Template** | `src/main.tsx`, `src/App.tsx`, `src/routes/**/*`, `public/*`, Tailwind, React Query. |
| **Routing** | `src/routes/foo.tsx` ⇢ `/foo` (mirrors Next App Router). |
| **Preview** | Sandpack + `esbuild-wasm` for **instant** feedback. |

---

## 3. Transform Service (Vite → Next.js)
1. **Unpack** archive.
2. **Skeleton**: inject `package.json`, `next.config.js`, `tailwind.config.*`, `app/layout.tsx`, `globals.css`.
3. **Route Conversion**: every `src/routes/**/file.tsx` ⇒ `app/**/page.tsx`.
4. **Entry**: `src/App.tsx` ⇒ `app/page.tsx`; drop `index.html` / `main.tsx`.
5. **Build**: `pnpm install && next build` inside Firecracker VM.
6. **Deploy**: Vercel API, Netlify CLI, or tarball.
7. **Respond**: stream logs + return URL.

Security: 20 MB project cap, `postinstall` scrubbed, VM GC after 30 min.

---

## 4. Twelve Expansion Tracks (Detailed)

### 4.1 Multi-Framework Preview
* **What**: Offer Vue & Svelte templates in Sandpack.  
* **Transform**: Vue/Svelte files routed through **Turbopack** in the wrapper monorepo; React root remains unchanged.  
* **Benefit**: Broader appeal; same deploy target.

### 4.2 Server-Component Upgrade Wizard
* **Scan** generated Next code for component purity (no browser-only APIs).  
* **Suggest** converting candidates to `"use server"` files.  
* **One-click** migrates & re-builds.

### 4.3 Auth/Billing Presets
* **Deploy modal checkboxes**: _Add Auth_, _Add Billing_.  
* Injects Auth.js, Stripe SDK, required API routes, `.env` placeholders.  
* **Transform** auto-configures keys via Secrets Manager.

### 4.4 AI-Assisted Transform Diff
* After build, show side-by-side tree (Vite vs Next).
* Claude API summarises significant code changes.  
* Users can accept/override before final deploy.

### 4.5 Custom CI/CD Export
* **Download** GitHub Action file replicating transform + deploy.  
* Lets teams own pipeline.

### 4.6 Live Database Playground
* Provision Neon branch per preview.  
* Transform injects `schema.prisma` & `.env`.
* **UI panel** for SQL console + seed.

### 4.7 Component Library Publishing
* Detect `/src/components` → package, version & push to private npm feed.  
* Semantic release triggered on deploy.

### 4.8 Rollback & Versioning
* Snapshot each build artefact.  
* **Dashboard dropdown** to roll back or promote previous versions.

### 4.9 Plugin Marketplace
* Community-authored transform hooks (i18n, Sentry, image-optimisation).  
* Marketplace UI & permission prompts.

### 4.10 Edge-Function Performance Testing
* Synthetic latency & cold-start tests against build preview.  
* Fail-fast thresholds configurable in Deploy modal.

### 4.11 Team Collaboration
* Live cursors & comments in Sandpack (Yjs).  
* Protected branches for transform products; PR review inside dashboard.

### 4.12 Usage Analytics Hook-In
* Toggle PostHog/Amplitude snippets.  
* Transform injects env vars & initialisation code.

---

## 5. Delivery Phases
| Phase | Timeline | Milestones |
|-------|----------|------------|
| **1** | 2 wks | Core transform + Vercel deploy |
| **2** | 1 wk  | Netlify deploy, live logs UI |
| **3** | 2 wks | Dynamic routes, Tailwind polish, env var UI |
| **4** | 3 wks | Additions 4.3, 4.6, 4.8 |
| **5** | Ongoing | Remaining roadmap items, plugin marketplace |

---

## 6. Risks & Mitigations
| Risk | Impact | Mitigation |
|------|--------|-----------|
| Transform edge-cases | Build failure | Extensive unit tests, static analysis |
| Vendor API rate-limits | Deploy delays | Caching, exponential back-off |
| Rising infra cost (Phase 3+) | $$$ | Idle VM shutdown, concurrency caps |

---

## 7. Glossary
* **Builder** – The Sandpack iframe users edit code in.
* **Transform Service** – Backend micro-service performing Vite→Next conversion.
* **Preset** – Optional feature pack (Auth, Billing, etc.) toggled at deploy time.

---

> Drafted automatically by Cascade. Edit freely before publishing.


