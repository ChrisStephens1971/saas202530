# Irrigation/HVAC: Lightweight Scheduler + Offline Mobile

**Project Type:** Wedge MVP (4–6 weeks)  
**Primary Buyer:** Service manager / owner  
**Target Users:** Dispatch, field techs  
**Problem Statement:** Legacy tools are expensive and brittle; techs lose data when signal drops.

---

## 1) Objective
Build a narrow, high-ROI MVP that removes the most expensive step in the workflow, without rip-and-replace. Ship in 4–6 weeks with production quality.

## 2) Pain Signals (from the field)
- Frequent outages
- Per-seat pricing pain
- Offline forms fail

## 3) Core Jobs-To-Be-Done
- Fast scheduling without bloat
- Offline capture with reliable sync
- Priced per crew, not per seat

## 4) Wedge MVP Scope (what ships in 4–6 weeks)
- Calendar board with drag‑drop jobs
- Offline mobile job forms + queued sync
- Per‑crew pricing/timesheets
- Customer notifications
- Basic reporting

**Out of scope for MVP (build later):**
- Inventory/parts
- Quotes/estimates
- Advanced routing

## 5) 60‑Day ROI Metrics (must improve or kill it)
- Downtime incidents down 70%
- Job completion rate up 10%
- License cost cut by 30–50%

## 6) Pricing Hypothesis (launch)
- Pilot: $129/mo org, up to 3 crews
- Pro: $349/mo org, up to 30 crews
- Ops: $699/mo org, unlimited users + priority support

## 7) Key Workflows (E2E)
1. Dispatcher schedules jobs; techs see daily list
2. Techs capture photos/notes offline; sync later
3. Customers receive ETA SMS
4. Timesheets roll up by crew

## 8) Core Entities & ERD Notes
- Entities: Job, Crew, Customer, Visit, Timesheet
- Relationships: Crew 1‑N Visits; Visit 1‑N Photos; Job 1‑N Visits
- Multi‑tenant isolation by `org_id`; all queries scoped.

## 9) Integrations (MVP)
- SMS, email, storage

## 10) Architecture Blueprint (AI can scaffold)
- Web: Next.js 14+ (App Router), React Server Components, Tailwind
- API: Node/TS (tRPC or REST) or Go (Fiber/Chi) behind a gateway
- DB: PostgreSQL (Prisma), row‑level security by `org_id`
- Auth: Passwordless/email + OAuth; organization membership & roles
- Realtime: Pusher/Ably (pub/sub) where needed
- Background jobs: durable queue (BullMQ or Cloud tasks)
- Files: S3‑compatible blob storage (signed URLs)
- Notifications: Transactional email + SMS provider
- Payments: Stripe (standard or Connect as noted)
- Hosting: Cloud provider with per‑env stacks; infra as code
- Observability: structured logs, traces, error tracker
- Feature flags: simple boolean flags in DB
- Admin: internal admin panel with audit log

## 11) Data Model Highlights
- `organizations(id, name, plan, …)`
- `users(id, email, …)`
- `memberships(user_id, org_id, role)`
- Domain entities: jobs, crews, customers, visits, timesheets
- All tables include `org_id`, `created_at`, `updated_at`, `actor_id`

## 12) Security & Compliance
- RBAC: owner, manager, staff (expand per domain)
- Audit trail on create/update/delete of domain entities
- PII minimization; encryption at rest & TLS in transit
- Rate limits on public endpoints; signed URLs for files
- Least‑privilege DB roles; weekly offsite backups

## 13) Analytics & Telemetry
- Product analytics funnels on the top 3 workflows
- Business KPIs: the ROI metrics above, per org and cohort
- Export CSVs and webhook for BI tools

## 14) Launch Plan (Weeks 1–6)
- Week 1: confirm scope with 3 pilot customers; finalize schema; scaffold repo
- Week 2: implement domain entities and primary workflow
- Week 3: notifications + payments + basic admin
- Week 4: polish UX; seed data; write acceptance tests
- Week 5: onboard pilots; instrument metrics; fix field bugs
- Week 6: prove ROI; decision to expand or stop

## 15) Risks & Go/No‑Go Gates
- Offline sync edge cases; SMS compliance
- No‑Go if pilots don’t hit the ROI metrics by day 60

## 16) Acceptance Tests (MVP)
- Tech completes 20 jobs with zero data loss despite poor signal.

## 17) Sample User Stories
- As a manager, I can see crew utilization per day and week.

## 18) API Surface (outline)
- POST /auth/sign‑in
- POST /orgs
- CRUD /jobs
- CRUD /visits
- POST /webhooks/sync-complete
- GET  /reports/uptime

## 19) Backlog After MVP
- Advanced routing, quotes, parts inventory

---

**Notes for the AI:** keep tenancy airtight, ship the smallest workflow that proves money saved or revenue recovered, collect evidence by default (timestamps, deltas, before/after). Do not gold‑plate. 
