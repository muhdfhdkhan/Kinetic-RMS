# Kinetic RMS — Recruitment Management System

A production-grade **Recruitment Management System (RMS)** that combines Applicant Tracking (ATS), Client Relationship Management (CRM), and an on-device **AI talent engine** in a single web application.

Kinetic RMS digitizes the entire recruitment lifecycle — from client mandates and candidate sourcing to submissions, interviews, offers, and placements — while automating operational follow-up (tasks, reminders, notifications) and communications (email / SMS / WhatsApp / webhook templates) through an automation-rule engine.

---

## Why this software exists — value in the industry

Recruitment firms run on **pipeline visibility and speed**. Kinetic RMS answers the questions a staffing business asks daily:

- **Where is every candidate, mandate, submission, and offer at this moment?** — a live dashboard with pipeline funnel, status breakdowns, and 6-month activity trends.
- **Which candidates should we put in front of a client?** — an AI matching engine scores candidates against job requirements with a transparent breakdown (skills, experience, location, notice period, title fit) and human-readable reasons.
- **Are our recruiters productive?** — recruiter performance reports, top-recruiter ranking, and activity tracking per user.
- **Are we losing candidates between stages?** — funnel conversion percentages reveal exactly where pipeline drops happen.
- **What needs my attention today?** — interviews today, overdue tasks, pending offers, and upcoming reminders are surfaced on the dashboard.
- **Do we stay compliant and auditable?** — full audit logging, role-based access, and exportable reports.

For A recruitment firm this translates into **faster time-to-fill, higher placement conversion, enforceable SLA follow-up, and defensible data** for clients and management.

---

## Features

### 1. Recruitment pipeline (ATS)
- **Candidates** — full lifecycle (NEW → SOURCED → SUBMITTED → INTERVIEWING → OFFERED → HIRED / REJECTED), duplicate detection on name/email/phone, skills, tags, notes, work experience, education, resume uploads (PDF & DOCX), and attachment history.
- **Jobs & Mandates** — client mandates with required/preferred skills, experience ranges, location, salary bands, statuses (DRAFT → OPEN → ON_HOLD / CLOSED), and shortlists.
- **Submissions** — candidate-to-job submissions with expected/proposed salary, recruiter notes, and client feedback.
- **Interviews** — multi-round scheduling per submission with type, meeting links, status, and real-time **ICS calendar sync**.
- **Offers & Placements** — offer management (pending/accept/decline) flowing into placements with joining dates and final salary; placements feed reporting and the dashboard funnel.

### 2. AI talent engine — 100% on-device
No external AI APIs, no API keys, no data leaves the server. Built on local parsing and deterministic scoring:
- **Resume parsing** — extracts contact details, skills, experience blocks, education, certifications, and languages from PDF/DOCX, auto-triggered on upload.
- **Apply parsed profile** — one click writes the structured profile back to the candidate record.
- **Candidate summaries** — auto-generated professional summaries.
- **Job matching** — scores each candidate against a mandate (skills, experience, location, notice period, title fit) with a **transparent breakdown and reasons**; verdicts: EXCELLENT / GOOD / POSSIBLE / POOR.
- **Job requirement parsing** — build structured requirements from a free-text job description.
- **Screening** — bulk screen a shortlist in one call.
- **Skill-gap analysis** — missing/preferred-skill gaps per candidate vs. a job.
- **Semantic search** — ranked keyword search across the entire candidate pool (TF-IDF).
- **Recommendations** — candidates-for-job, jobs-for-candidate, and similar candidates.

### 3. CRM & clients
- Client companies with contacts, notes, tags, and statuses (PROSPECT → ACTIVE → …).
- Per-client mandate history, placements, and communication trail.

### 4. Automation & integrations
- **Communication channels** — Email, SMS, WhatsApp, and Webhook transports with identity settings (name, from-address, reply-to, signature).
- **Automation rules** — event-driven templates (e.g. interview scheduled → email reminder to the candidate) with rich `{{variable}}` interpolation (nested paths such as `{{candidate.name}}`, `{{interview.meetingLink}}`) and per-rule delivery testing.
- **Delivery log** — every simulated message is recorded; channel transports run in **simulation mode** by default so no real messages are sent during development.
- **Calendar** — create/import events, export to **ICS**, and sync scheduled interviews to the calendar.

### 5. Operations
- **Tasks & Follow-ups** — assignments, priorities, due dates, statuses, and overdue detection.
- **Reminders** — scheduler-driven alerts surfaced in-app and via notifications.
- **Notifications** — unread badge, read/unread tracking.
- **Communication log** — every touchpoint with a candidate or client; **Recruiter activity** feed.
- **Saved searches** — persist and replay frequently used filters.

### 6. Reporting & insight
- **Dashboard** — KPI tiles (candidates, open mandates, submissions, placements, interviews today, pending offers/tasks, overdue tasks), **pipeline funnel with conversion %**, 6-month activity trend (submissions/interviews/placements), top recruiters of the month, status breakdowns, upcoming interviews, recent activity, and "needs attention" shortcuts.
- **Entity reports** — candidates, clients, jobs, submissions, interviews, placements with KPIs, monthly trends, and breakdowns by status/source/location/owner.
- **Recruiter performance** — per-recruiter KPI comparison.
- **CSV export** of every report; **CSV import** for candidates/clients/jobs.

### 7. Client portal
- Dedicated login for client companies: their own dashboard, mandates, candidate shortlists, interview schedule, contact directory, and notifications — without exposing the internal system.

### 8. Administration & security
- **RBAC** — granular permission keys across 20+ modules; four seeded roles: **ADMIN** (77 permissions), **MANAGER** (55), **RECRUITER** (46), **VIEWER** (20).
- **Users & roles management**, organization profile, application settings.
- **Audit logs** — who did what, when, on which record.
- **Security defaults** — Helmet headers, rate limiting, JWT cookie auth (HTTP-only), Zod validation, CORS allow-listing, bcrypt password hashing.

---

## Tech stack

| Layer | Technology |
| --- | --- |
| Backend | Node.js (ESM), Express, Prisma ORM, MySQL 8 |
| Frontend | React, Vite, Tailwind CSS, Zustand, React Router, lucide-react, lottie-react |
| Auth | JWT (access + refresh) in HTTP-only cookies, RBAC middleware |
| Data handling | MySQL 8, Prisma migrations & seeding |
| Files | Multer uploads (resumes/attachments) |
| Parsing | pdf-parse (PDF), mammoth (DOCX) — fully local |
| Integrations | SMTP draft (simulated), ICS calendar, webhook simulation |
| Observability | pino structured logging (HTTP request logs) |
| Docs | Seed data + this README |

---

## Project structure

```
├── server/                    # REST API (port 4000)
│   ├── prisma/
│   │   ├── schema.prisma      # data model
│   │   ├── migrations/        # SQL migrations
│   │   └── seed.js            # permissions, roles, admin, demo data
│   ├── src/
│   │   ├── config/            # env, prisma, multer, cors
│   │   ├── routes/            # feature route modules
│   │   ├── controllers/       # request/response handling
│   │   ├── services/          # business logic (auth, candidates, ai, …)
│   │   ├── middleware/        # auth, RBAC, validation, error, rate-limit
│   │   ├── utils/             # logger, permission keys, csv, ics
│   │   ├── scheduler.js       # reminder/task scheduler
│   │   └── index.js           # entry point
│   └── uploads/documents/     # uploaded resumes
└── client/                    # SPA (port 5173)
    └── src/
        ├── api/               # axios modules per feature
        ├── components/        # UI primitives + layout
        ├── constants/         # labels/status maps
        ├── layouts/           # app shell + navigation
        ├── pages/             # feature pages (incl. dashboards & portals)
        ├── stores/            # auth & toast state (zustand)
        └── assets/            # Lottie animation assets
```

---

## Getting started

### Prerequisites
- **Node.js 20+** (tested on v22)
- **MySQL 8+** running locally (service `MySQL80`), credentials of your choice
- Git (optional)

### 1. Configure the database & environment

```bash
cd server
cp .env.example .env        # then edit
```

Minimum changes in `server/.env`:

```env
DATABASE_URL="mysql://root:yourpassword@localhost:3306/kinetic_rms"
JWT_ACCESS_SECRET=change-me-access-secret-32chars-min
JWT_REFRESH_SECRET=change-me-refresh-secret-32chars-min
ADMIN_EMAIL=admin@kinetic.local
ADMIN_PASSWORD=Admin@123
```

### 2. Install, migrate, seed

```bash
cd server
npm install
npm run prisma:generate
npm run prisma:migrate       # first run: creates the schema (migrate dev)
npm run seed                 # permissions, roles, admin user + demo data
```

`npm run seed` is idempotent — safe to re-run after permission changes.

### 3. Run the API

```bash
cd server
npm run dev                  # http://localhost:4000  (auto-restarts on change)
```

On first boot you should see `Database connection established` and `Kinetic RMS API listening on http://localhost:4000`.

### 4. Run the frontend

```bash
cd ../client
npm install
npm run dev                  # http://localhost:5173
```

The Vite dev server proxies `/api` to the API, so **only the frontend URL is used in the browser**.

### 5. Production build (optional)

```bash
cd client
npm run build                # emits dist/ (static assets)
npm run preview              # serve the build locally
```

---

## Default access

| Role | Credentials | Scope |
| --- | --- | --- |
| Administrator | `admin@kinetic.local` / `Admin@123` (as set in `.env`) | Everything (77 permissions) |
| Manager / Recruiter / Viewer | created via **Admin → Users** or seeded demo accounts | 55 / 46 / 20 permissions |

Portal test accounts (seeded by `npm run seed:portal-test`):

| Portal | Credentials | Profile |
| --- | --- | --- |
| Candidate (`/portal/login`) | `candidate@local.com` / `M@12345m` | **100% complete** — 8 skills, 2 jobs, 1 degree, resume, summary, salary & availability |
| Client (`/client/login`) | `client@local.com` / `M@12345m` | Complete company profile (NovaTech Solutions FZ-LLC) + 2 hiring contacts |
| Admin (staff) | `admin@kinetic.local` / `Admin@123` | Full CRM access at `/login` |

All REST endpoints sit under `/api` and authenticate via HTTP-only cookies (set by `POST /api/auth/login`).

---

## Using the system

### Daily recruiter flow
1. **Add a candidate** (`Candidates → Add`) and upload the resume — the system **auto-parses** it into a structured profile.
2. Open the candidate's **AI Profile** tab to review extracted skills/experience, generate a summary, or apply the parsed profile with one click.
3. **Create a job mandate** on a client and paste the description — use **AI → Parse job** to auto-build requirements, then **Run AI match** on the job to rank candidates with scores and reasons.
4. **Submit** the best candidate, schedule **interviews** (rounds + type + link), and track statuses in **Submissions**.
5. When the client accepts, raise an **offer**, then convert to a **placement** once the candidate signs.

### Automation in one minute
1. Open **Integrations → Automation Rules**.
2. Create a rule: *when* `INTERVIEW_SCHEDULED` *send* `EMAIL` to the candidate using a template with `{{candidate.name}}`, `{{interviewDate}}`, `{{interview.meetingLink}}`.
3. Press **Test rule** — a simulated delivery appears in the **Delivery Log** (no real messages are sent; flip a channel to live transport when production-ready).

### Reports & oversight
- **Dashboard** — funnel conversion, monthly trends, top recruiters, and "needs attention".
- **Reports** — per-entity reports with trends and breakdowns; **Export** to CSV; **Recruiter Performance** for team comparisons.
- **Admin → Audit Logs** for a complete, searchable history of changes.

---

## API conventions

- Base URL: `http://localhost:4000/api` (proxied automatically from the client at `:5173`)
- All endpoints: `JSON` bodies, `{ success, data }` (or `{ success, message }`) responses
- Auth: HTTP-only JWT cookies (`kinetic_access` 15 min, `kinetic_refresh` 7 days, silent refresh)
- Access control: `requirePermission(PERMISSIONS.X)` middleware on every route
- Pagination: `?page=1&pageSize=25` on list endpoints
- Errors: `{ success: false, message, details? }` with appropriate HTTP status codes

---

## Scripts reference

| Command | Where | Purpose |
| --- | --- | --- |
| `npm run dev` | server | Start API with auto-restart on file changes |
| `npm start` | server | Start API (production mode) |
| `npm run prisma:migrate` | server | Apply schema migrations |
| `npm run prisma:generate` | server | Regenerate Prisma client |
| `npm run seed` | server | Idempotent permission/role/admin/demo seeding |
| `npm run dev` | client | Vite dev server (HMR) |
| `npm run build` | client | Production bundle |
| `npm run preview` | client | Serve the production build |

---

## Notes & conventions

- **AI is intentionally local** — resume parsing, summaries, matching, and search run on-device; no third-party AI service is contacted and no candidate data leaves the server.
- **Communication channels ship in simulation mode** — perfect for demos and development; nothing is actually emailed or messaged until transport is enabled.
- The **Loading 40 – Paperplane** Lottie animation (in `client/src/assets/animations/`) plays during page-to-page navigation.
- Audit events are recorded for create/update/delete/export actions across modules.

---

Kinetic RMS — built and maintained by **National Tech Forge**.
