# CI & CD Workshop — Detailed Slide and Workshop Plan

## Working title

**From localhost to production: practical CI/CD and deployment for student projects**

## Target audience

UNSW students with:

* frontend-only projects
* full-stack web apps
* some backends in TypeScript or Python
* limited budget and limited ops experience

## Core message

For most student projects, the best default is:

* **Frontend:** Vercel
* **Backend:** Vercel Functions or Cloudflare Workers
* **Database:** Supabase by default, or Cloudflare data products if they want to stay fully on Cloudflare
* **Domain / DNS:** Cloudflare
* **CI/CD:** GitHub Actions + preview deployments + branch protection + tests/lint/format

Do **not** start with a traditional always-on server unless the project really needs stateful long-running behavior.

## Learning outcomes

By the end of the workshop, students should be able to:

1. Explain the difference between CI and CD.
2. Explain serverless vs server, and stateless vs stateful.
3. Choose a practical deployment architecture for a small personal project.
4. Understand where Vercel, Cloudflare Workers, Supabase, D1, KV, Durable Objects, and R2 fit.
5. Set up a minimal CI pipeline with lint/build/test.
6. Understand branch protection and preview deployments.
7. Understand when Docker is useful and when it is not the default answer.
8. Understand how to attach a custom domain.

## Delivery format

* **Total time:** 1 hour 50 minutes to 2 hours
* **Style:** lecture + live demos + short interactive decisions + optional hands-on deployment

## Recommended workshop structure

### Part 0 — Opening and framing (5 min)

### Part 1 — How to choose a hosting model (15 min)

### Part 2 — Recommended stack for student projects (15 min)

### Part 3 — Serverless vs server / stateless vs stateful (15 min)

### Part 4 — Frontend, backend, and full-stack deployment paths (20 min)

### Part 5 — Databases and storage (20 min)

### Part 6 — CI/CD basics that matter in practice (20 min)

### Part 7 — Domains, DNS, and custom domains (10 min)

### Part 8 — Docker and traditional clouds in context (10 min)

### Part 9 — Hands-on exercise / guided deployment (20–30 min)

---

# Slide-by-slide plan

## Slide 1 — Title

**From localhost to production**
Subtitle: **Practical CI/CD and deployment for student projects**

Speaker notes:

* This workshop is not about enterprise DevOps theory.
* It is about how to ship student projects with minimal cost and minimal operations burden.
* We will focus on practical defaults, not every possible cloud product.

## Slide 2 — Who this workshop is for

* Frontend-only apps
* Next.js apps
* Python or TypeScript backends
* CRUD apps, dashboards, portfolios, small tools
* Students who want something online without managing servers all day

Speaker notes:

* If your project is unusual, the same decision framework still applies.
* We are optimising for simplicity, cost, and speed.

## Slide 3 — Main recommendation upfront

**Default recommendation:**

* Vercel for frontend
* Vercel Functions or Cloudflare Workers for backend
* Supabase for SQL/auth/storage by default
* Cloudflare if you want one ecosystem for DNS/domain/edge/backend/storage

Speaker notes:

* Say the answer early, then justify it through the workshop.
* This helps students orient themselves.

## Slide 4 — Why this recommendation

* scales to zero
* low cost for personal projects
* less ops work
* easy preview deployments
* easy custom domains
* fast to ship

Speaker notes:

* Most students do not need to run a VM.
* If the app sleeps when nobody uses it, that is often fine.

---

## Slide 5 — Interactive: choose a deployment path

Show 4 example projects:

1. Portfolio site
2. React app with auth and CRUD
3. Next.js full-stack app
4. Python backend with background or realtime behavior

Ask students:

* Which one should use only frontend hosting?
* Which one should use serverless?
* Which one might actually need a stateful backend?

Speaker notes:

* Let people vote quickly by hand or poll.
* Use this to motivate the next section.

## Slide 6 — First principle: choose the hosting model before the provider

* static frontend
* full-stack serverless
* frontend + managed backend services
* stateful server backend

Speaker notes:

* The provider choice is secondary.
* First decide what kind of system you are deploying.

## Slide 7 — Serverless vs server

Two-column slide:

**Serverless**

* code runs on demand
* scales down when idle
* lower idle cost
* good default for student projects

**Server**

* process stays alive
* easier for some long-running/stateful cases
* costs money while idle
* more ops responsibility

Speaker notes:

* Use concrete language, not cloud marketing.
* Explain that “serverless” still runs on servers, just not your server to manage.

## Slide 8 — Stateless vs stateful

Two-column slide:

**Stateless**

* each request can go anywhere
* no shared in-memory state assumed
* easier to scale
* default for Vercel Functions / Workers

**Stateful**

* memory / process / local attached state matters
* harder to restart or scale
* sometimes needed for specific workloads

Speaker notes:

* Give examples: user sessions should live in DB or storage, not only in process memory.
* Mention chat rooms / collaborative state as examples where stateful coordination matters.

## Slide 9 — Architecture rule of thumb

* Keep compute stateless if possible
* Put data in managed storage
* Only choose stateful app servers when the product actually requires it

Speaker notes:

* This is the practical design rule students should remember.

---

## Slide 10 — Frontend recommendation

**Frontend first choice: Vercel**
Why:

* simple for React / Next.js
* strong preview deployment story
* easy custom domain setup

**Alternative:** Cloudflare Workers

* good if you want frontend + backend + storage + DNS all on Cloudflare

Speaker notes:

* Be explicit that Workers is now also a frontend option.
* Do not recommend Cloudflare Pages for new projects; mention it only briefly if needed.

## Slide 11 — Why not Cloudflare Pages for new projects?

* not deprecated
* but Workers is the stronger default now for new full-stack projects on Cloudflare
* one model for static assets + APIs + platform integrations

Speaker notes:

* Keep this short and practical.
* Do not spend time on historical product evolution.

## Slide 12 — Backend recommendation

**First choice:** Vercel Functions
**Second choice:** Cloudflare Workers

Good for:

* API routes
* auth callbacks
* lightweight backend logic
* Python or TypeScript endpoints

Speaker notes:

* Say clearly that both are good.
* Vercel is the easier first step for many students; Workers is great if they want Cloudflare-first.

## Slide 13 — Next.js is a special case

**Next.js can include both frontend and backend in one project**

* pages / app router / server logic
* API routes / server actions depending on app design
* can often be deployed as one unit

Speaker notes:

* Students often think frontend and backend must always be separate repos.
* Explain that Next.js changes that.

## Slide 14 — Next.js deployment choices

* **Vercel:** easiest default
* **Cloudflare Workers:** possible using the OpenNext adapter

Speaker notes:

* Keep this concrete.
* If students want “all-in-one app + easy deployment”, Next.js on Vercel is a very strong default.
* If they want Cloudflare, explain OpenNext as the adapter that makes Next.js run on Workers.

## Slide 15 — Interactive: choose between separate frontend/backend vs Next.js

Ask:

* If your app is mostly UI with a few API needs, would you keep two repos?
* When is a single Next.js project simpler?
* When is a separate backend still cleaner?

Speaker notes:

* Encourage students to think in tradeoffs, not ideology.

---

## Slide 16 — Databases and storage: this is where state should live

Split into:

* SQL
* key-value / NoSQL-ish
* object storage
* stateful coordination

Speaker notes:

* Reframe databases as part of architecture, not an afterthought.

## Slide 17 — SQL default: Supabase

Why Supabase is a strong default:

* Postgres
* auth included
* storage included
* can simplify smaller projects a lot

Speaker notes:

* This is especially good when students do not want to write a separate backend yet.

## Slide 18 — Important Supabase nuance

**Supabase can simplify frontend-only projects**

* use frontend SDK
* use auth
* use Row Level Security
* keep access controlled even without your own backend for basic cases

Speaker notes:

* This is the key nuance to emphasise.
* Explain that this can be a perfectly reasonable architecture for small projects.

## Slide 19 — Cloudflare SQL option: D1

Use when:

* you want SQL
* you want to stay in Cloudflare
* you are building with Workers already

Speaker notes:

* Position D1 as Cloudflare-native serverless SQL.
* Do not oversell it as universally better than Supabase.

## Slide 20 — NoSQL / KV

Introduce Cloudflare KV as:

* simple key-value storage
* useful for config, feature flags, cached data, simple session-like patterns

Speaker notes:

* Clarify that not all data needs relational structure.
* Keep NoSQL introduction light.

## Slide 21 — Stateful coordination: Durable Objects

Use when:

* multiple clients need shared coordinated state
* chat rooms
* multiplayer rooms
* collaborative editing style patterns

Speaker notes:

* Explain that this is not just a database.
* It is a stateful compute primitive.

## Slide 22 — Object storage: R2

Use when:

* file uploads
* images
* PDFs
* videos
* user-generated assets

Speaker notes:

* Say clearly that object storage is different from SQL.
* R2 is the Cloudflare suggestion here.

## Slide 23 — Interactive matching exercise

Show scenarios and ask students to match:

* user profiles and posts → SQL
* feature flags → KV
* uploaded images → R2
* a shared room with many connected users → Durable Objects

Speaker notes:

* This is an easy and memorable exercise.

---

## Slide 24 — When you might need a stateful backend server

Examples:

* long-running background processes
* libraries/frameworks that assume a persistent process
* certain websocket / connection-heavy designs
* workloads that do not fit serverless constraints well

Speaker notes:

* The point is not “never use servers.”
* The point is “do not start there by default.”

## Slide 25 — Free-ish stateful backend options

Mention briefly:

* Render
* maybe Koyeb if you want one more example

Caution:

* free tiers are limited
* cold starts / spin-down behavior
* less suitable as default architecture

Speaker notes:

* Mention Render specifically because students may ask.
* Say it still exists as an option, but not your default recommendation.

## Slide 26 — Why we do not recommend AWS / GCP / OCI as the default here

* powerful
* important in industry
* but too much complexity for many student personal projects
* easy to overbuild and harder to control cost

Speaker notes:

* Touch them respectfully, but keep them in context.
* This workshop is about practical defaults, not résumé theatre.

---

## Slide 27 — What CI is

**Continuous Integration**

* run checks when code changes
* lint
* format checks
* build
* tests

Speaker notes:

* CI reduces regressions.
* It is now also important when using AI coding tools.

## Slide 28 — What CD is

**Continuous Delivery / Deployment**

* automatically deploy after checks pass
* or make deployment consistently repeatable

Speaker notes:

* Explain delivery vs deployment briefly if you want, but do not overdo it.

## Slide 29 — Why CI is not only for humans anymore

* AI can write code faster
* that also means AI can introduce regressions faster
* CI gives a safety net
* tests, linting, and build checks help harness AI productively

Speaker notes:

* This is a strong modern framing.
* Keep it practical, not philosophical.

## Slide 30 — What should be in a student CI pipeline?

Minimum:

* install dependencies
* lint
* format check
* type-check if applicable
* build
* tests

Speaker notes:

* This should be a checklist slide.

## Slide 31 — Linters and formatters matter

Examples:

* ESLint
* Prettier
* Ruff / Black for Python
* TypeScript type-checking

Speaker notes:

* Emphasise that this is not just about style.
* It makes automation and collaboration easier.

## Slide 32 — Branch protection matters

Recommend:

* protect main
* require pull requests
* require status checks to pass
* optionally require at least one review

Speaker notes:

* This is one of the highest-value practices students can adopt quickly.

## Slide 33 — GitHub Actions and GitLab CI

* GitHub Actions: likely most relevant for personal projects
* GitLab CI: COMP1531 students may already know the idea
* same big idea: pipeline defined in YAML, triggered by repo events

Speaker notes:

* Keep GitLab mention short and comparative.
* Do not teach two CI systems in depth.

## Slide 34 — Very simple workflow example

Show a minimal GitHub Actions file:

* checkout
* setup runtime
* install
* lint
* test
* build

Speaker notes:

* Keep example small enough to read live.
* Avoid advanced matrix examples unless you have extra time.

## Slide 35 — One practical warning about workflows

* do not casually allow PR-triggered deployment workflows from everyone
* simplest safe advice for student repos: keep that restricted unless you know exactly what you are doing

Speaker notes:

* Keep this very brief.
* Do not turn this into a security lecture.

## Slide 36 — Preview environments are one of the best parts of modern CD

* every PR can get a deployment preview
* reviewers can click and see the change
* less need for a dedicated staging server on small projects

Speaker notes:

* This is a major selling point for Vercel and Cloudflare-based workflows.

## Slide 37 — Demo: open a preview deployment

Show:

* main / production URL
* preview / PR URL
* how this changes review workflow

Speaker notes:

* A live demo is best here if possible.

---

## Slide 38 — Domains and DNS

Explain simply:

* a domain is your address on the internet
* DNS points that address to your hosting provider
* using your own domain looks better than provider subdomains

Speaker notes:

* Do not get stuck in DNS internals.

## Slide 39 — Recommend Cloudflare for domain / DNS

* Cloudflare Registrar
* Cloudflare DNS
* easy to connect custom domains to Vercel or Cloudflare

Speaker notes:

* Position Cloudflare as the practical default recommendation.

## Slide 40 — Brief note on email

* you can use custom addresses on your domain with Cloudflare Email Routing
* mention it only as incoming routing / forwarding
* do not go deep into full email hosting or outbound mail

Speaker notes:

* Keep this to under a minute.

---

## Slide 41 — Docker: what students should know

* Docker exists
* useful for local development
* useful for databases and reproducible environments
* useful when deploying to container-based platforms
* not the default deployment answer for most personal projects

Speaker notes:

* This is a corrective slide.
* Last year Docker felt too central; here it should be contextual.

## Slide 42 — My view on Docker vs mise for local dev

* Docker is still good, especially for DBs
* but local dev environment sharing can also be solved with tools like mise
* choose the simplest thing that fits your team/project

Speaker notes:

* This is where you can insert your personal preference without making it dogma.

## Slide 43 — Self-hosting / k8s / vendor lock-in

* Docker can help portability
* it matters more on traditional cloud or self-hosted setups
* this is not where most student projects should start

Speaker notes:

* Keep it balanced.

---

## Slide 44 — Decision tree summary

A simple flowchart:

1. Is it mostly frontend? → Vercel
2. Need backend logic? → Vercel Functions or Workers
3. Using Next.js? → one project on Vercel, or Workers via OpenNext
4. Need auth + DB quickly without much backend? → Supabase
5. Want everything on Cloudflare? → Workers + D1/KV/R2
6. Need long-running stateful server? → consider Render, but not default

Speaker notes:

* This is one of the most important slides in the deck.

## Slide 45 — Architecture examples

Example 1:

* React SPA on Vercel
* Supabase Auth + Postgres

Example 2:

* Next.js app on Vercel
* built-in server features
* Supabase for DB/storage

Example 3:

* Worker on Cloudflare
* D1 + KV + R2
* custom domain on Cloudflare

Example 4:

* frontend on Vercel
* Python API on Cloudflare Workers or Vercel Functions

Speaker notes:

* Concrete examples make the recommendations real.

## Slide 46 — Interactive architecture clinic

Ask students to classify their own project idea:

* frontend only?
* full-stack in one repo?
* separate backend?
* auth needed?
* file uploads?
* realtime/shared state?

Speaker notes:

* Let them spend 2–3 minutes deciding their likely stack.

## Slide 47 — Workshop hands-on brief

Options:

* deploy a static frontend
* add a serverless function
* connect a DB
* enable CI pipeline
* add custom domain if time allows

Speaker notes:

* Decide ahead of time whether you will do one canonical app or multiple tracks.

## Slide 48 — Suggested canonical hands-on app

Use one small app such as:

* simple todo / issue tracker / links app
* frontend on Vercel
* optional serverless API
* optional Supabase DB
* CI pipeline on GitHub Actions

Speaker notes:

* Keep the app boring; the infrastructure is the learning target.

## Slide 49 — Hands-on steps

1. push code to GitHub
2. connect repo to Vercel
3. deploy and confirm URL
4. add env vars if needed
5. add CI workflow
6. protect main branch
7. create PR and preview deployment
8. optionally attach custom domain

Speaker notes:

* This becomes the exercise checklist.

## Slide 50 — Final advice

* start simple
* avoid running servers unless you need them
* let CI catch regressions
* use preview deployments
* use your own domain when possible
* optimise for shipping and iteration, not cloud complexity

Speaker notes:

* End with principles, not products.

## Slide 51 — Q&A / project clinic

Invite students to ask:

* what stack fits their project
* when to use Workers vs Vercel
* when Supabase is enough
* when a real server is justified

---

# Demo plan

## Demo 1 — Deploy a frontend to Vercel

Time: 5–7 min

* connect repo
* build automatically
* show production URL

## Demo 2 — Add a preview deployment

Time: 3–5 min

* open a PR
* show preview URL
* compare to main deployment

## Demo 3 — Show a minimal CI workflow

Time: 5 min

* `.github/workflows/ci.yml`
* lint/test/build
* see failing and passing checks

## Demo 4 — Show a Cloudflare Worker full-stack option

Time: 5–7 min

* mention static assets + Worker API
* if possible show one tiny example app structure

## Demo 5 — Optional Next.js deployment comparison

Time: 5 min

* explain Vercel direct deployment
* briefly show OpenNext/Workers path conceptually

---

# Interactive exercises

## Exercise A — Architecture voting

Students vote on the right deployment strategy for 4 example projects.

## Exercise B — Storage matching

Students match SQL / KV / R2 / Durable Objects to use cases.

## Exercise C — CI checklist review

Show a repo and ask what is missing:

* tests?
* linter?
* formatter?
* branch protection?
* preview environment?

## Exercise D — Project clinic

Students outline their own stack in pairs or small groups.

---

# Optional workshop tracks

## Option 1 — Mostly lecture + demos

Good if time is short or room setup is unreliable.

## Option 2 — Guided hands-on

Everyone deploys a small app during the session.

## Option 3 — Hybrid

Lecture first, then students choose:

* Vercel + Supabase track
* Cloudflare Workers track
* Next.js track

Recommended: **Hybrid**

---

# What to cut if time is short

Cut in this order:

1. traditional clouds detail
2. Docker detail
3. second stateful backend example
4. deeper NoSQL nuance
5. email mention

Do not cut:

* hosting model explanation
* Vercel/Workers recommendation
* Supabase nuance
* CI/CD basics
* preview deployments
* branch protection
* domains/DNS mention

---

# Suggested final narrative

1. Most student projects do not need a traditional server.
2. Serverless + managed data is usually the right starting point.
3. Vercel is the easiest general default.
4. Cloudflare Workers is a strong alternative, especially if you want everything on Cloudflare.
5. Next.js is a useful full-stack special case.
6. Supabase is excellent when frontend-only auth/data access simplifies the project.
7. CI/CD is how you make iteration safe, especially when using AI.
8. Docker exists, but it is not the main story for most students here.
9. A custom domain makes your project feel real.

---

# Suggested speaker prep checklist

* Have one Vercel demo repo ready
* Have one Cloudflare Worker example ready
* Have one minimal GitHub Actions workflow ready
* Have one preview deployment already generated in case live demo fails
* Have one architecture decision slide printable as a handout
* Decide whether hands-on will use starter repos or student repos

---

# Optional appendix slides

* Vercel vs Workers comparison table
* Supabase vs D1 quick comparison
* Next.js on Vercel vs Workers with OpenNext
* Render caveats
* Basic custom domain setup flow
* sample GitHub Actions YAML
* minimal branch protection rule setup

---

# One-sentence summary for the deck

**Teach students to choose the simplest deployment architecture that ships fast, costs little, and stays maintainable: Vercel first, Workers when Cloudflare-first makes sense, Supabase for simpler frontend-auth/data apps, and stateful servers only when truly required.**
