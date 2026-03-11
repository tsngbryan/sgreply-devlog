# SGReply Session Log
_Most recent session at the top. Updated at the end of each working session._

---

## 2026-03-11 — SEO, learn blog, devlog viewer

### Done
- SEO/GEO foundations on sgreply.com — sitemap, robots.txt, canonical tags, JSON-LD schemas
- learn.sgreply.com blog launched — SSL live, all 3 articles rendering, clean URLs fixed
- DevLog viewer built — animated site at tsngbryan.github.io/sgreply-devlog showing this log
- Session log system set up — auto-updates at end of each session, synced to public devlog repo

### Pending
- Fix sgreply.com/sitemap.xml returning HTML instead of XML (Lovable CDN catch-all issue)
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
- WhatsApp settings hub on frontend (Lovable prompt written, pending implementation)
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