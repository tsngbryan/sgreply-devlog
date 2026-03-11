# SGReply Session Log
_Most recent session at the top. Updated at the end of each working session._

---

## 2026-03-11 — SEO/GEO foundations, learn site fixes, sitemap issue

### Done
- **SEO/GEO implemented on sgreply.com (Lovable):** robots.txt with AI bot allowlist (GPTBot, PerplexityBot, ClaudeBot), sitemap index + sitemap-main.xml covering all 8 public routes, canonical tags via SeoHead component on every route, JSON-LD Organization + SoftwareApplication + FAQPage schemas, H1 audit across all public pages
- **learn.sgreply.com articles fixed:** Two markdown files were missing `.md` extension — renamed. Missing `layout: article.njk` frontmatter added to use-cases article. Permalink casing fixed on scaling article. All 3 articles now build and render correctly
- **Double /learn/learn/ URL resolved:** Replaced redirect approach with host-aware static middleware in `src/index.js` — `learn.sgreply.com` now serves blog at `/` root cleanly
- **SSL cert provisioned for learn.sgreply.com:** Second required DNS record (TXT verification) added in Porkbun; Railway issued Let's Encrypt cert automatically
- **Sitemap issue diagnosed:** `sgreply.com/sitemap.xml` returns HTML instead of XML because Lovable's CDN catch-all intercepts static files. Plan: Lovable hosting config fix + Railway cross-domain fallback at `learn.sgreply.com/sgreply-sitemap.xml`

### Pending
- **Fix Lovable hosting config** so `/sitemap.xml` returns XML — Lovable prompt: add `public/_redirects` (Netlify) or `vercel.json` rewrite (Vercel) to serve static files before SPA catch-all
- **Add Railway cross-domain sitemap** — add `GET /sgreply-sitemap.xml` route in `src/index.js` serving XML with proper Content-Type; submit URL directly in Google Search Console
- **Submit both sitemaps to Google Search Console and Bing Webmaster Tools** — `sgreply.com` and `learn.sgreply.com` as separate properties
- **Update learn site header** to match sgreply.com nav (Features, How It Works, Pricing, Demo links)
- **Add image support to article cards** on learn.sgreply.com index + rename listing page slug from `/` to `/articles`

---

## 2026-03-09 — WhatsApp manual connect, pdf-parse fix, knowledge base limits

### Done
- **WhatsApp manual connect endpoint built** (`POST /api/whatsapp/connect-manual`): accepts `phoneNumberId` + `accessToken`, verifies token against Meta Graph API, best-effort WABA ID lookup, upserts into `whatsapp_numbers` with `ON CONFLICT (client_id)`
- **Webhook config endpoint added** (`GET /api/whatsapp/webhook-config`): returns `webhookUrl` and `verifyToken` so clients know exactly what to paste into Meta Business Manager — critical missing piece for onboarding
- **pdf-parse crash fixed on Railway** — v2.x used PDF.js with browser DOM globals (`DOMMatrix`) that don't exist in Node.js. Downgraded to `pdf-parse@1.1.1` (plain async function export). Updated `package.json` and `services/document.js`
- **Knowledge base file limits updated** — new upload constraints applied to `routes/documents.js`
- **Multi-tenant WhatsApp architecture confirmed** — webhook routing already looks up `phone_number_id` from `whatsapp_numbers` table, supports multiple clients with separate WABA accounts

### Pending
- ~~learn.sgreply.com blog setup~~ ✅ done in next session
- WhatsApp onboarding integration settings hub (Lovable prompt written, pending frontend implementation)
- Email/SMS auth confirmations (M1 remaining ~20%)
- Client message approval flow for low-confidence AI replies

---

## 2026-03-08 — Railway deployment stabilised

### Done
- **Railway deployment healthcheck fixed** — raised `healthcheckTimeout` from 30s to 120s in `railway.toml` to allow time for DB + Redis + OpenRouter init
- **Infisical skipped on Railway** — detection via `RAILWAY_ENVIRONMENT` env var; secrets loaded from Railway dashboard directly instead
- **Webhook verify token hardened** — moved from placeholder to `WHATSAPP_VERIFY_TOKEN` env var; added debugging middleware
- **`railway.toml` created** — `buildCommand: npm install && npm run build`, `startCommand: npm start`, `healthcheckPath: /`

### Pending
- ~~pdf-parse crash~~ ✅ fixed 2026-03-09
- ~~WhatsApp connect endpoint~~ ✅ built 2026-03-09

---

## 2026-03-06 / 2026-03-07 — Railway initial deployment

### Done
- **Deployed to Railway** — Express app live, PostgreSQL and Redis provisioned, environment variables set in Railway dashboard
- **Database connection fixed for Railway** — SSL enabled (`rejectUnauthorized: false`), connection string via `DATABASE_URL` env var
- **Bull queue operational** — `REDIS_URL` connected, worker processing messages at concurrency 5
- **Health check endpoint** (`GET /`) returns bot status and current model list

### Pending
- ~~Healthcheck timeout too short~~ ✅ fixed 2026-03-08
- ~~Infisical running in production (wrong)~~ ✅ fixed 2026-03-08

---

## 2026-02-17 to 2026-02-21 — M1 Foundation

### Done
- **Core message pipeline built** — WhatsApp webhook → message batcher (5s window) → Bull queue → AI model racer → confidence threshold → send/draft
- **OpenRouter multi-model racing** — `raceModels()` fires parallel tracks through `FREE_MODELS` list, first valid ≥10 char response wins; winning model promoted in-memory cache
- **Knowledge base upload** — PDF and DOCX parsing via `pdf-parse` and `mammoth`, stored in `knowledge_documents`, injected into AI system prompt
- **Dynamic quick-reply buttons** — AI generates contextual WhatsApp button options from knowledge base, cached in `client_quick_actions`
- **Frontend (Lovable) scaffolded** — dashboard with `/knowledge-base`, `/settings`, `/conversations`, login/signup, onboarding wizard; Supabase auth integrated end-to-end

### Pending
- ~~Railway deployment~~ ✅ done 2026-03-06
- Full M1 remaining: email/SMS auth confirmations, client message approval flow
- M2: multi-tenant, WhatsApp Flows, human handoff, analytics, i18n