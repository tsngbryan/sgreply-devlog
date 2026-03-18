# SGReply Session Log
_Most recent session at the top. Updated at the end of each working session._

---

## 2026-03-18 — 4-agent content pipeline, learn SEO, brand consistency

### Done
- Built 4-agent headless content pipeline (research → writer → reviewer → publisher) using `claude -p` subprocesses; `claude-runner.js` resolves binary from VS Code/Cursor extension dirs
- Generated and published first 2 articles: PSG grant WhatsApp automation + Instagram DM for blogshop sellers
- Fixed Eleventy `learn/` setup: collection glob, passthrough copy paths, pathPrefix, `| url` filter on all links, strip-visuals transform
- Fixed learn sitemap URLs (now `/learn/slug/` not `/slug/`); added second `Sitemap:` line in robots.txt; removed manual article entries from main sitemap
- Added keywords meta, BreadcrumbList JSON-LD, FAQPage JSON-LD to `article.njk`; `faq_schema` flows from writer agent → frontmatter YAML → structured data
- Replaced "SGReply" → "SG Reply" in all external-facing display text: learn templates, article .md files, all 5 agent prompts, client/src Landing/Login/Demo/PublicFooter/ChatPanel/Settings components; URLs and #SGReply hashtag preserved

### Pending
- Build `client/dist` and deploy to Railway with brand name + article changes live
- Run `/content` pipeline for article 3 (next theme rotation)
- Submit `https://sgreply.com/learn/sitemap.xml` to Google Search Console as additional sitemap
- Monitor Google indexing of `/learn/psg-grant-*/` and `/learn/instagram-dm-*/`
- Check Eleventy build output on Railway to confirm `_site/sitemap.xml` generates correctly

---

## 2026-03-17 — KB progress bar, verified facts, templates, MCP setup, auth & deploy fixes

### Done
- Added `client_facts` table (migration 008), `/api/facts` CRUD routes, and injected verified facts as a high-priority "VERIFIED FACTS" block in the AI system prompt — prevents hallucination of prices, links, addresses
- KnowledgeBase dashboard rebuilt with tabs (Response Gaps / Verified Facts / Change History), storage progress bar, template download link, and connected sources section; Advanced Settings collapsible wraps the facts editor
- Created `client/public/templates/knowledge-base-template.txt` — downloadable KB template for onboarding users
- Onboarding step 3 updated: per-file size limit display, template download link, file count + KB total progress bar (total size validation removed — backend enforces text limit)
- Fixed Railway build chain: `railway.toml` eleventy step was aborting the client build; fixed `npm run build` script and build command so `client/dist` is always generated
- Diagnosed and fixed `waba_id` column missing error — migration 002 instructions provided for Railway Postgres console; also flagged migrations 007 (kb undo) and 008 (client facts) as pending
- Google OAuth "invalid_request" diagnosed — OAuth app in Testing mode blocking second accounts; advised publishing via Google Cloud Console (all required fields + privacy policy URL needed)
- Supabase email verification link whitelabelled — Supabase email template updated to point to `sgreply.com/auth/confirm?token_hash=...`; AuthCallback handles token exchange
- Postgres MCP server configured in `~/.claude/settings.json` for direct Railway DB access in future sessions

### Pending
- Run migration 002 (waba_id columns), 007 (kb_edit_log raw_text_before), 008 (client_facts) on Railway Postgres console
- Restart VS Code again to activate postgres MCP server (settings.json was updated this session)
- Publish Google OAuth app — fill all required fields in OAuth consent screen then publish to allow any Google account to sign in
- Evolution API personal WhatsApp not receiving messages — `evolution_connections` table is empty despite QR scan; root cause not yet fully diagnosed (migration 011 may not have run)
- widget.js: replace purple circle with pill "Support" button flushed to screen bottom
- KB editor `findSection` still returns wrong paragraph for PDF documents — position-based keyword clustering fix was planned but not yet implemented
- KB undo feature (migration 007) implemented in code but not yet deployed/tested end-to-end
- `client_quick_actions` cache not cleared on document upload/delete — stale buttons served after KB swap

---

## 2026-03-12 — Frontend migrated to Railway, devlog photo refresh

### Done
- Frontend migrated from Lovable to Railway — React/Vite now lives in `client/`, served by Express
- sgreply.com now points to Railway (Porkbun ALIAS record updated)
- Health check moved from `GET /` to `GET /api/health`; railway.toml updated
- Sitemap HTML-instead-of-XML issue resolved — no more SPA catch-all interfering
- DevLog viewer refreshed — full-page background images, 90 location scenes, no animations

### Pending
- Submit sitemaps to Google Search Console and Bing Webmaster Tools
- Update learn site header to match sgreply.com navigation
- Add article card images + rename listing page to /articles
- Verify frontend auth flows work correctly post-migration (same-origin API calls)

---

## 2026-03-11 — SEO, learn blog, devlog viewer

### Done
- SEO/GEO foundations on sgreply.com — sitemap, robots.txt, canonical tags, JSON-LD schemas
- learn.sgreply.com blog launched — SSL live, all 3 articles rendering, clean URLs fixed
- DevLog viewer built — animated site at tsngbryan.github.io/sgreply-devlog showing this log
- Session log system set up — auto-updates at end of each session, synced to public devlog repo

### Pending
- ~~Fix sgreply.com/sitemap.xml returning HTML~~ ✅ resolved by Railway migration 2026-03-12
- Submit sitemaps to Google Search Console and Bing Webmaster Tools
- Update learn site header to match sgreply.com navigation
- Add article card images + rename listing page to /articles

---

## 2026-03-09 — WhatsApp onboarding, knowledge base

### Done
- WhatsApp manual connect — clients can now paste their Meta credentials to link their number
- Webhook config endpoint — exposes the verify token and URL clients need for Meta Business Manager
- Knowledge base PDF upload fixed — was crashing on Railway due to wrong pdf-parse version
- Multi-tenant WhatsApp architecture confirmed working

### Pending
- ~~learn.sgreply.com blog~~ ✅ done 2026-03-11
- WhatsApp settings hub on frontend (pending implementation in `client/`)
- Email/SMS auth confirmations (M1 remaining)
- Client message approval flow for low-confidence replies

---

## 2026-03-08 — Railway deployment stabilised

### Done
- Railway healthcheck timeout fixed — app now has enough time to fully start
- Secrets management sorted — Infisical skipped in production, Railway env vars used instead
- Webhook verification token secured

### Pending
- ~~PDF knowledge base crash~~ ✅ fixed 2026-03-09
- ~~WhatsApp connect endpoint~~ ✅ done 2026-03-09

---

## 2026-03-06 / 2026-03-07 — First Railway deployment

### Done
- App deployed to Railway — Express, PostgreSQL, Redis all live
- Database SSL connection configured for production
- Bull queue connected and processing messages

### Pending
- ~~Healthcheck fixes~~ ✅ done 2026-03-08

---

## 2026-02-17 to 2026-02-21 — M1 Foundation

### Done
- Core WhatsApp message pipeline — webhook to AI reply, end to end
- AI model racing — multiple free models compete, fastest valid response wins
- Knowledge base upload — PDF and DOCX parsed and used in AI replies
- Dynamic quick-reply buttons — AI generates contextual WhatsApp buttons from knowledge base
- Frontend dashboard scaffolded — knowledge base, settings, conversations, auth all working

### Pending
- ~~Railway deployment~~ ✅ done 2026-03-06
- M1 remainder: email/SMS auth confirmations, message approval flow
- M2: multi-tenant, human handoff, analytics, WhatsApp Flows, i18n