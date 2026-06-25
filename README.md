<span style="color: red;">*SAS Platform — 3-Month Pre-Development Planning Roadmap — confidential*</span> 
<span style="color: green;">**This is a draft document - can be modified anytime**</span>  


---

>  [navigate_to_stack](STACK.md)

# MONTH 1 — Research & Foundational Decisions
### Weeks 1–4 | Understand the problem deeply before touching a single line of architecture

Most projects fail not because of bad code but because of wrong assumptions baked in early. Month 1 is about eliminating assumptions. Every question you answer now prevents a week of rework in Month 4.

---

## Week 1 — Know Your User

Before you build anything, deeply understand who is going to use this and what their daily pain looks like. This week is interviews and observation — not documents.

### Research: Teacher Workflow Audit

- What does a teacher's end-to-end assessment workflow look like today? (setting the question → marking → entering the grade)
- How many students does a typical teacher assess per week? Per term? Per year?
- What file formats do they already work with — are papers handwritten or typed?
- Where do they currently store results — Excel, Google Sheets, school ERP software?
- What is the single most time-consuming part of their assessment process?
- Do they share results with parents, admin, or other teachers? How exactly?
- Are they comfortable uploading files to a web tool or do they prefer desktop software?
- Would they use this at home, at school, or both? What devices — phone, tablet, laptop?
- What would make them pay for a tool like this? What would make them refuse?

### Research: Competitive Landscape

- What tools do teachers currently use for grading? (Gradescope, Google Forms, Turnitin, iGradePlus)
- What do teachers hate about existing tools? Read reviews on Product Hunt, Reddit r/Teachers, App Store
- What does Gradescope charge? What does Turnitin charge? What is included at each tier?
- What do free tools offer vs paid tools? Where is the gap your product fills?
- Are there India-specific EdTech tools solving this? (local market — Teachmint, Classplus, etc.)
- What integrations do teachers already expect — Google Classroom, MS Teams, Canvas?

### Output to be Deliverable by end of Week 1
A **1-page Teacher Persona document**. Name, school type, tech comfort level, biggest pain, what success looks like for them. Both developers must agree on this persona — it guides every single design decision for the next 3 months.

---

## Week 2 — Product Scope Decision

The most dangerous thing for a two-person team is scope creep. This week you make hard decisions about what is in v1 and what is not. Everything excluded from v1 goes on a backlog, not in the bin.

### Feature Scope Table

| Feature | v1 Decision | Reason |
|---|---|---|
| Answer sheet evaluation | **Core — keep** | Teachers need this most. Already built. Refine it. |
| OMR bubble sheet grading | **Core — keep** | Unique differentiator. Already built. Strong foundation. |
| Plagiarism / AI detection | **Relabel & keep** | Useful but must be honest — it is style analysis, not source checking. |
| Class & student management | **v1 simplified** | Teachers need somewhere to organise students and past results. |
| Batch / bulk upload | **v1 basic** | Teachers have 30 students. Uploading one at a time is a dealbreaker. |
| Job status & email notification | **v1 basic** | Tell the teacher when a batch job is done. No staring at a spinner. |
| Report export (Excel + PDF) | **v1 basic** | Teachers need to share results with admin. Export is non-negotiable. |
| Student progress tracking | **v2** | Valuable but complex. Needs historical data first. |
| Google Classroom / Canvas sync | **v2** | OAuth approval takes weeks. Do not block v1 on it. |
| Student-facing result portal | **v3** | Big scope addition. Teachers first, students later. |
| Mobile app | **v3** | Web-first. Mobile is a separate product decision. |
| Multi-school admin dashboard | **v3** | Needed for institutional sales but not for a soft launch. |

### Output to be Deliverable by end of Week 2
A **signed-off v1 feature list**. Both developers write their name next to it. No adding features to v1 after this without removing something else of equal or greater scope.

---

## Week 3 — Tech Stack Validation (Hands-On)

You have discussed the stack. This week you validate it hands-on. Not just reading docs — actually running code. Both developers complete every task independently so you both know the stack.

### Hands-on Validation Tasks 

- Spin up a FastAPI app with one POST endpoint. Send a file upload to it from curl. How long did setup take?
- Create a Next.js project from scratch. Build a file upload form. Does it feel manageable?
- Run Postgres in Docker locally. Connect from Python with SQLAlchemy. Create a table, insert a row, query it.
- Run Redis in Docker. Write a Celery task in Python. Confirm it runs asynchronously in a worker process.
- Set up Keycloak in Docker. Create a user. Log in. How painful was the setup? Was it worth it vs simpler options?
- Run MinIO in Docker. Upload a file via the Python SDK (boto3). Retrieve it. Download it. Does the API feel natural?
- Run Tesseract on a sample answer sheet image. Compare extracted text quality vs Google Vision output side by side.
- Run Ollama locally with Llama 3. Send a plagiarism-style prompt. Compare output quality and latency vs Gemini.
- Write a Dockerfile for your FastAPI hello-world. Build it. Run the container. Confirm it behaves identically.
- Write a docker-compose.yml with FastAPI + Postgres + Redis. Run `docker compose up`. All three talking to each other.

### Output to be Deliverable by end of Week 3
An **Architecture Decision Record (ADR)** for each major tech choice.

Format per ADR:
```
## Decision: [technology name]
**What we chose:** ...
**What else we considered:** ...
**Why we picked this:** ...
**Known trade-offs:** ...
```

Store in `../assets/adrs/`. This is your future reference when you forget why you made a choice.

---

## Week 4 — Domain, Accounts & Legal Groundwork

Boring but critical. These items take time to resolve and block other work if left until later.

### Task Checklist

| Task | Action | Cost |
|---|---|---|
| Domain name | Buy yourdomain.com on Porkbun or Namecheap. Check .com and .in availability. | ~$10/year |
| Docker Hub | Create an org account under your product name. Create two repos: `sas-frontend`, `sas-backend`. | Free |
| GitHub organisation | Create a GitHub org (not personal). Set up the monorepo with `/frontend`, `/backend`, `/docs`. | Free |
| Hetzner account | Sign up, add payment method, spin up CX22 VPS, SSH in, install Docker, then tear it down. | €0.01 test |
| Product email | Set up hello@yourdomain.com via Zoho Mail (free) or Cloudflare Email Routing. | Free |
| DPDP Act research | India's Digital Personal Data Protection Act — what does it require for handling student data? | Research only |
| Privacy policy draft | What data do you collect? How long do you keep it? Who can access it? Draft, do not publish yet. | Research only |
| Terms of service draft | What are users agreeing to? What are you not liable for? Draft, do not publish yet. | Research only |
| Analytics decision | Plausible Analytics (open source, self-hostable) vs Google Analytics. Pick one and document why. | Research only |

### Output to be Deliverable by end of Week 4
All accounts created, domain purchased, VPS tested and torn down. Legal drafts started. You now have the full infrastructure scaffolding ready to use in Month 3.

---
---

# MONTH 2 — Architecture & System Design
### Weeks 5–8 | Design the system on paper so completely that coding becomes filling in the blanks

Month 2 converts your research and decisions into precise, agreed blueprints. Every diagram and document produced this month is something a new developer could read and understand the system without asking you anything.

---

## Week 5 — Database Schema Design

Your database schema is the skeleton of the entire product. Get it wrong and you will be writing painful migrations for years. Get it right and every feature builds naturally on top.

### Tables to Design — every column, type, constraint, and relationship

- **users** — `id (UUID)`, `email`, `name`, `role (teacher/admin)`, `org_id (FK)`, `created_at`, `last_login_at`, `is_active`
- **organisations** — `id`, `name`, `type (school/individual/district)`, `plan_tier`, `created_at`, `billing_email`
- **classes** — `id`, `org_id (FK)`, `teacher_id (FK)`, `name`, `subject`, `academic_year`, `student_count (cached)`
- **students** — `id`, `class_id (FK)`, `name`, `roll_number`, `email (optional)`, `is_active`
- **submissions** — `id`, `class_id (FK)`, `teacher_id (FK)`, `type (answer_eval/omr/plagiarism)`, `status`, `created_at`
- **submission_files** — `id`, `submission_id (FK)`, `minio_object_key`, `file_type`, `size_bytes`, `original_filename`
- **reports** — `id`, `submission_id (FK)`, `student_id (FK)`, `score`, `total_marks`, `percentage`, `details_json`, `generated_at`
- **jobs** — `id`, `submission_id (FK)`, `celery_task_id`, `status (queued/running/done/failed)`, `error_message`, `started_at`, `finished_at`
- **audit_log** — `id`, `user_id (FK)`, `action`, `resource_type`, `resource_id`, `ip_address`, `user_agent`, `timestamp`

### Critical Questions to Answer for Every Table

- What is the primary key strategy — UUID or auto-increment integer? *(UUID recommended — no enumeration attacks)*
- Which columns get database indexes? Every FK and every column used in a WHERE clause needs one.
- What gets soft-deleted (`is_deleted` flag + `deleted_at` timestamp) vs hard-deleted? Student data: soft-delete only.
- What is your data retention policy? How long do uploaded files live in MinIO? How long do reports stay?
- What columns are nullable vs NOT NULL? Write it explicitly in your schema diagram.

### Output to be Deliverable by end of Week 5
An **Entity-Relationship Diagram (ERD)** built in draw.io (free at diagrams.net). Every table, every column, every relationship with cardinality. Export as PNG and PDF. Store in `../assets/architecture/`. Both developers sign off — this is your database contract for v1.

---

## Week 6 — API Contract Design

Design every API endpoint before writing a single line of FastAPI code. This is the contract between frontend and backend. Once agreed, both developers can work in parallel without blocking each other.

### Endpoint Inventory

| Method + Path | Purpose | Auth |
|---|---|---|
| `POST /auth/login` | email + password → JWT access + refresh tokens | Public |
| `POST /auth/google` | Google OAuth callback → JWT tokens | Public |
| `POST /auth/refresh` | Refresh token → new access token | Refresh token |
| `GET /me` | Current logged-in user profile and organisation | JWT required |
| `GET /classes` | List all classes for the current teacher | JWT required |
| `POST /classes` | Create a new class | JWT required |
| `POST /classes/:id/students` | Bulk upload student roster via CSV file | JWT required |
| `POST /submissions/evaluate` | Upload answer sheets + key → enqueue job → return job_id | JWT required |
| `POST /submissions/omr` | Upload OMR sheets + answer key image → enqueue job → return job_id | JWT required |
| `POST /submissions/plagiarism` | Upload assignment files → enqueue job → return job_id | JWT required |
| `GET /jobs/:id/status` | Poll job progress: queued / running / done / failed | JWT required |
| `GET /reports/:id` | Fetch completed report with all scores and details | JWT required |
| `GET /reports/:id/export` | Download report as Excel or PDF (`?format=xlsx\|pdf`) | JWT required |
| `GET /classes/:id/reports` | All reports for a given class, paginated | JWT required |
| `DELETE /submissions/:id` | Delete a submission and its associated files | JWT required |

### For Every Endpoint, Document These Four Things

1. **Request shape** — exact field names, data types, required vs optional, file size limits
2. **Success response shape** — exact JSON structure, every field, what it means
3. **Error responses** — every HTTP status code it can return (400, 401, 403, 404, 422, 500) and the error message format
4. **Rate limiting** — should this endpoint be throttled? File upload and AI endpoints definitely should be.

### Output to be Deliverable by end of Week 6
An **`openapi.yaml` spec file** stored in `../assets/api/`. FastAPI generates this automatically later, but writing it manually first forces you to think precisely. Use [editor.swagger.io](https://editor.swagger.io) to validate it renders correctly.

---

## Week 7 — User Flow & UI Wireframes

Design every screen the teacher will see. Not visually polished — just structure, buttons, information hierarchy, and states. Use **Excalidraw** (free, open source) or Figma's free tier.

### Screens to Wireframe — every screen, every state, annotated

- **Landing page** — what does a teacher see before signing up? What is the value proposition in one sentence?
- **Sign up / Login** — email+password form AND a prominent Google SSO button
- **Onboarding wizard** — first-time user: create organisation → create class → upload student roster
- **Dashboard** — logged-in home: recent submissions, class overview, quick actions. What if there is nothing yet (empty state)?
- **New submission wizard** — step-by-step: choose type (eval/OMR/plagiarism) → upload files → configure settings → submit
- **Job status / progress page** — what does the teacher see while OCR and AI processing runs? Progress bar? Estimated time?
- **Report view** — individual student score, class average, per-question breakdown, colour-coded pass/fail
- **Export screen** — choose format (Excel/PDF), choose what to include, download button
- **Class management** — student roster table, add/remove students, view all past submissions for the class
- **Settings** — profile details, change password, notification preferences
- **Error states** — what does the teacher see when a job fails? When a file format is unsupported? When the server is down?
- **Empty states** — dashboard with no classes, class with no submissions, report list with no reports

### Annotate Every Wireframe With

- What data is displayed here and where does it come from (which API endpoint)?
- What happens when the user clicks each interactive element?
- What are the loading states — what does the user see while data fetches?
- What are the error states — what does the user see when something goes wrong?

### Output to be Deliverable by end of Week 7
A wireframe for every screen above, stored in Excalidraw and exported as PNG into `/docs/wireframes/`. Share the Excalidraw link so both devs can comment. Do not proceed to Month 3 until both developers are happy with every screen.

---

## Week 8 — System Architecture Diagrams

Three diagrams that every person on the team should be able to read and explain from memory. Build in draw.io and store in `../assets/architecture/`.

### Diagram 1 — Deployment Architecture

- Every Docker container, labelled with its image name and port number
- Which containers are exposed to the internet (only Caddy) and which are internal-only
- How Caddy routes traffic: `yourdomain.com` → frontend, `yourdomain.com/api` → backend
- Docker internal network — which containers can talk to which
- Docker volumes — which container writes to which volume, what data lives there
- External API calls — which container calls Gemini API, which calls Google Vision, on what triggers

### Diagram 2 — Request Lifecycle (happy path, end-to-end)

1. Teacher uploads 30 PDFs via the Next.js frontend form
2. Frontend sends `POST /api/submissions/evaluate` with files to FastAPI
3. FastAPI validates request, stores files in MinIO, creates submission + job records in Postgres
4. FastAPI enqueues a Celery task via Redis and immediately returns `{ job_id }` to the frontend
5. Frontend polls `GET /api/jobs/:id/status` every 3 seconds showing a progress indicator
6. Celery worker picks up the task, fetches files from MinIO, runs OCR → text extraction → similarity scoring
7. Worker writes results to Postgres `reports` table, updates job status to `done`
8. Frontend receives `done` status, navigates teacher to the full report view
9. Teacher clicks export → FastAPI generates Excel from database → file streams to browser

### Diagram 3 — Security & Data Boundary Map

- What data is encrypted in transit? *(Everything — Caddy enforces HTTPS on all connections)*
- What data is encrypted at rest? *(Postgres volume, MinIO storage — document the encryption config)*
- Which ports are open on the VPS firewall? *(Only 80 and 443 — nothing else, ever)*
- Where are secrets stored? *(.env on VPS, GitHub Secrets for CI/CD, never in Git)*
- What happens to uploaded files after a report is generated? Define and document the retention policy.
- How are JWT tokens validated — which services check them and by what mechanism?
- What personal data flows through the system? Map every field that identifies a real person.

### Output to be Deliverable by end of Week 8
All three diagrams in draw.io, exported as PNG, added to `/docs/architecture/`. These are **living documents** — update them whenever the architecture changes. A diagram that is out of date is worse than no diagram.

---
---

# MONTH 3 — Specifications, Standards & Launch Prep
### Weeks 9–12 | Write the rules you will code by so Month 4 has zero ambiguity

Month 3 converts your architecture blueprints into precise implementation specifications. Every ambiguity resolved now is a debugging session you will never have to have.

---

## Week 9 — Feature Specifications

A spec answers: *exactly* how does this feature behave? Not what it does in broad terms — the precise rules, edge cases, and failure modes. Write one spec document per major feature area.

### Answer Evaluation Spec — every question answered in plain English

- What similarity threshold counts as a correct answer? Is 70% similarity full marks? Who configures this — teacher or system default?
- How are partial marks handled? Is 60% similarity worth 50% of the marks, or zero?
- What happens when a student answer is blank? Zero marks, or flagged separately for manual review?
- What file formats are accepted? What is the maximum file size? What happens if exceeded?
- What is the maximum number of students per batch submission?
- How is the score rounded — to 2 decimal places, or whole numbers? Who decides?
- Can a teacher re-run evaluation with a different answer key? What happens to the previous report?
- Does the report show the matched text as evidence, or just the score?

### OMR Grading Spec — every question answered in plain English

- What bubble sheet format is supported? Fixed template or configurable by teacher?
- Maximum questions per sheet? Maximum options per question (A–D or A–E)?
- Minimum image quality required? (DPI, lighting, orientation tolerance before the system gives up)
- How are ambiguous bubbles handled — two bubbles filled, or a bubble only partially darkened?
- Is negative marking supported? Is it configurable per exam (e.g. −0.25 per wrong answer)?
- When the system cannot read a sheet confidently — fail with an error, or flag for manual review?
- How does the teacher provide the answer key — another OMR sheet, or a UI form?

### Background Job Processing Spec

- What is the maximum number of concurrent Celery workers on a Hetzner CX22 VPS?
- What is the timeout for a single job? What happens when a job exceeds the timeout?
- How many automatic retries on failure? With what backoff — immediate, 30s, 5 min?
- How does the teacher get notified on completion — email, in-app notification, or both?
- How long are job logs and error messages retained in the database?
- How do you prevent one teacher's 100-file batch from starving another teacher's 5-file job?
- What happens if the Celery worker crashes mid-job — does the job resume, restart, or fail?

### Output to be Deliverable by end of Week 9
One spec document per feature stored in `/assets/specs/`. Plain English, no code. These become your acceptance criteria when writing tests in Month 4.

> If you cannot write the spec, you do not understand the feature well enough to build it yet.

---

## Week 10 — Coding Standards & Repository Structure

Agree on how you write code before you write any. Two developers with different habits produce a codebase that reads like two different products stitched together.

### Standards Table

| Standard | The Rule | Tooling |
|---|---|---|
| Python formatting | Black for auto-formatting on every save. No arguments — run the tool, accept the output. | `pip install black` |
| Python linting | Ruff for linting and import sorting. Treat all warnings as errors in CI. | `pip install ruff` |
| JavaScript formatting | Prettier with default config. Run on every save. No manual formatting debates. | `npm install prettier` |
| JS linting | ESLint with Next.js recommended config. No disabling rules without a comment explaining why. | Built into Next.js |
| Git branch strategy | `main` = production only. `dev` = integration. `feature/ticket-name` = individual work. | GitHub branch rules |
| Pull request rule | No self-merging ever. Every PR needs the other developer to review and approve. | GitHub branch protection |
| Commit messages | Conventional Commits: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:` prefix always. | commitlint in CI |
| Environment variables | `.env.example` in repo with placeholder values. Real `.env` never committed. Document every variable. | Create `.env.example` now |
| Secret management | GitHub Actions Secrets for CI/CD. `.env` file on VPS only. Zero hardcoded credentials anywhere. | Audit before every deploy |
| Error handling | No bare `except: pass` in Python. Every caught exception must be logged with context. Use `structlog`. | Enforced in code review |
| Testing baseline | Every new function gets at least one unit test. Every API endpoint gets an integration test. | `pytest` + `httpx` |
| File naming | Python: `snake_case` everywhere. JS/TS: `PascalCase` for components, `camelCase` for functions. | Linter enforces this |

### Repository Structure

```
sas-platform/                    ← one GitHub repo
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
├── backend/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── app/
│       ├── main.py
│       └── modules/             ← your existing OCR, OMR, eval code
├── assets/
│   ├── adrs/                    ← Architecture Decision Records
│   ├── specs/                   ← Feature specifications
│   ├── architecture/            ← System diagrams
│   ├── wireframes/              ← UI wireframes (PNG exports)
│   └── api/                     ← openapi.yaml
├── docker-compose.yml           ← local development
├── docker-compose.prod.yml      ← production
├── .env.example
├── .gitignore
├── README.md
└── CONTRIBUTING.md
```

### Output to be Deliverable by end of Week 10
A **`CONTRIBUTING.md`** file in the root of your repo. Every standard in the table above, written out clearly. This is the onboarding document for any future developer you hire — write it as if explaining to someone who has never worked on this project.

---

## Week 11 — Security Checklist & Compliance

Security is not a feature you add at the end. These are decisions you make now and build around from day one.

### Security Decisions to Make, Document, and Build Around

- **JWT token expiry:** 1-hour access token + 7-day refresh token. Document the rotation logic.
- **Password policy:** Minimum 8 characters. Check against HaveIBeenPwned API on sign-up (free API available).
- **File upload validation:** Check MIME type server-side, not just file extension. Define an allowlist of accepted types.
- **File size limits:** Max per file and max per batch submission. Enforce in FastAPI before the file hits disk.
- **Rate limiting:** Requests per minute per IP for public endpoints, per user for authenticated ones. Use `slowapi`.
- **CORS policy:** Explicitly list allowed origins in FastAPI. Never use wildcard `*` in production.
- **SQL injection:** SQLAlchemy ORM prevents this by default. Document that raw SQL strings are banned in the codebase.
- **Temp file handling:** All temp files use UUID names, stored in `/tmp`, deleted immediately after processing.
- **Dependency scanning:** Enable GitHub Dependabot to alert on vulnerable packages in both frontend and backend.
- **Data residency:** Hetzner Finland/Germany. Is this acceptable for Indian schools under the DPDP Act? Research and document.
- **Student data classification:** What is PII in your system? Name, roll number, scores. Document handling rules for each field.
- **Audit logging:** Every create, update, delete action on student data must be logged with `user_id` and `timestamp`.

### Output to be Deliverable by end of Week 11
A **security checklist** in `/docs/security.md`. Before launch, every single item must be checked off by both developers. Add a column for *verified by* and *verified on date*. Treat this like a pre-flight checklist — non-negotiable.

---

## Week 12 — Soft Launch Preparation

The final week of planning is about removing every possible friction from Month 4. When you sit down to write the first line of production code, there should be zero setup left to do.

### Final Checklist

| Task | Detail | Owner |
|---|---|---|
| Monorepo skeleton | Create all folders. Add `README.md`, `CONTRIBUTING.md`, `.env.example`, `.gitignore`. | Both devs |
| Docker Compose skeleton | Write `docker-compose.yml` and `docker-compose.prod.yml` with all 7 services defined — even if images do not exist yet. | Both devs |
| CI/CD pipeline skeleton | Write `.github/workflows/deploy.yml` with build + push + SSH deploy steps. | Both devs |
| GitHub Projects board | Set up a Kanban board. Create issues for every v1 feature. Label: `backend`, `frontend`, `devops`, `docs`. | Both devs |
| Database migrations setup | Choose Alembic for Python. Create the initial migration file from your ERD. Do not run it yet. | Backend dev |
| Staging environment plan | Decide: separate staging VPS or Docker Compose on a different port locally? Document the exact flow. | Both devs |
| Soft launch criteria | Write exactly what *ready to show real teachers* means. List 5 specific things that must work before anyone outside your team sees it. | Both devs |
| Rollback procedure | If a deploy breaks production, what is the exact sequence of commands to roll back to the previous image tag? Write it. Test it. | Both devs |
| Feedback collection plan | How will your first pilot teachers give feedback? Typeform, WhatsApp group, email thread? Set it up now. | Both devs |
| First sprint planning | Month 4, Week 1: what are the exact tickets both developers will work on? Define done criteria for each ticket. | Both devs |

### Output to be Deliverable by end of Week 12
The repo exists with the full skeleton. Every planning document is in `../assets`. The GitHub Projects board has every v1 feature as a ticket with an owner and done criteria. Both developers know exactly what they are building on Day 1 of Month 4.

**Planning phase is complete.**

---
---

# Appendix — Reference & Resources

## Tools to Learn During the 3 Months

| Tool | What it is for | Where |
|---|---|---|
| draw.io | Free web-based diagramming for all architecture and flow diagrams | diagrams.net |
| Excalidraw | Open source whiteboard for wireframes and rough sketches | excalidraw.com |
| Swagger Editor | Validate and preview your OpenAPI spec before writing any code | editor.swagger.io |
| Docker Desktop | Run your full stack locally with one command | docker.com |
| TablePlus | Free GUI for browsing your local Postgres database | tableplus.com |
| Bruno | Open source API client (like Postman). Test FastAPI endpoints locally | usebruno.com |
| Conventional Commits | Commit message standard that makes git history readable | conventionalcommits.org |
| Plausible Analytics | Open source, privacy-friendly, self-hostable analytics | plausible.io |

---

## Learning Resources — What to Read and When

- **FastAPI docs** (fastapi.tiangolo.com) — Read the full tutorial, not just the intro. *Week 3*
- **Next.js Learn** (nextjs.org/learn) — Free interactive course. Complete chapters 1–12 minimum. *Week 3*
- **Docker Get Started** (docs.docker.com/get-started) — Complete all 8 parts hands-on, not just reading. *Week 3–4*
- **Celery First Steps** docs — Read 'First Steps with Celery' and 'Next Steps' in full. *Week 3*
- **SQLAlchemy 2.0 ORM tutorial** — Read before designing the schema. Understand relationships and lazy loading. *Week 5*
- **OWASP Top 10** (owasp.org) — Read all 10. Know what injection, broken auth, and IDOR mean. *Week 11*
- **Conventional Commits spec** — 5 minutes to read, used in every commit forever. *Week 10*
- **India DPDP Act 2023 summary** — Search for plain-English explainers. Read before Week 4.

---

## 12-Week Summary

| Week | Theme | Key Deliverable | Both devs agree on |
|---|---|---|---|
| W1 | User research | Teacher persona document | Who we are building for |
| W2 | Scope decision | v1 signed-off feature list | What is in and what is out |
| W3 | Stack validation | ADR documents | Every tech choice and why |
| W4 | Groundwork | Accounts + domain + legal drafts | Infrastructure scaffolding |
| W5 | Database design | ERD in draw.io | Every table and relationship |
| W6 | API design | openapi.yaml spec | Every endpoint shape and contract |
| W7 | UI wireframes | All screens in Excalidraw | Every screen, state, and flow |
| W8 | Architecture | 3 system diagrams | Deployment, lifecycle, security |
| W9 | Feature specs | Spec doc per feature | Exact rules and edge cases |
| W10 | Code standards | CONTRIBUTING.md | How we write and review code |
| W11 | Security | Security checklist doc | What we protect and how |
| W12 | Launch prep | Repo skeleton + Kanban board | Day 1 of Month 4 is unambiguous |

---