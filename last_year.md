# Localhost to Online — Deploying and Maintaining Your Personal Projects

**Presenters:** Alfredo Torres, Drew Feig, Ketut Susilo

If you haven't filled out the attendance form, please do — it really helps us out.

## Agenda

1. Introducing DevOps
2. All about CI/CD
3. Docker basics
4. Deploying an app together
5. Git best practices
6. Your turn!

---

## What is DevOps?

Instead of writing code and just "throwing it over the wall" to the operations team to deploy, both sides work together from the beginning.

**DevOps** is a way of working where developers (Dev) and operations (Ops) teams collaborate closely to build, test, and deliver software faster and more reliably.

- DevOps lifecycle *(slide visuals)*

---

## CI/CD

**Continuous Integration (CI)** — Automatically testing and combining (integrating) code changes into a shared codebase.

**Continuous Delivery / Deployment (CD)** — Automatically deploying code to a server after it passes all CI checks.

### What CI tools typically do

- Run tests so nothing breaks
- Check code style (lint)
- Merge code only if all checks pass

### Two levels of CD

- **Continuous delivery** — Code is ready to deploy anytime, but a human presses the final "Deploy" button.
- **Continuous deployment** — Code is deployed automatically after CI, with no manual step.

### Example pipeline (e.g. GitHub Actions)

Starts with automated testing and style checks (CI), then deployment (CD) triggered by a push to `main`.

### Why CI/CD matters

- Reduces human errors in deployment
- Automates build, test, and deployment
- Faster delivery of new features
- Better collaboration between teams
- Consistent, reliable releases
- Supports frequent integration of code changes

---

## Docker

Docker lets you package your app and everything it needs into a portable **container**, so it runs the same everywhere — your laptop, someone else's machine, or a server.

### Why use Docker?

- Fixes the "it works on my machine" problem
- Lightweight, fast, uses fewer resources than full VMs
- Runs apps in isolated environments
- Easier to scale applications
- Faster deployments

### How it works

1. Write a Dockerfile
2. Build a Docker image
3. Upload the image to a container registry
4. Run a container from the image

---

## Workshop: Let's deploy an app

**The app:** *(GitHub repos as shown in slides)*

### Prerequisites (overview)

| Step | Topic |
|------|--------|
| 0 | Code in a Git repository (e.g. NestJS backend) |
| 1 | Cloud database |
| 2 | CI pipeline |
| 3 | Dockerfile |
| 4 | Deploy backend |
| 5 | Deploy frontend |
| 6 | Custom domain *(optional)* |

### Step 0: Git repository

Make sure your codebase is in a Git repository (e.g. NestJS backend).

### Step 1: Cloud database

Move your database to the cloud.

**SQL providers (examples):** Neon, Supabase, AWS RDS, Oracle DB  
**NoSQL (examples):** MongoDB, etc.

Connecting to Neon DB *(as in workshop slides).*

### Step 2: CI pipeline

A CI pipeline runs when code is pushed. It checks that the app still works and meets quality standards before merge or deploy.

**Typical CI steps:**

- Linting
- Build
- Testing
- Notifications *(optional)*

**GitHub Actions workflow (`.yml`):**

- Defines when the pipeline runs (`on:`)
- `runs-on:` — which runner VM to use
- Sets up Node.js and installs dependencies
- Caches `node_modules` to speed up runs
- Runs build; can upload **artifacts** for other jobs (e.g. deploy) or download
- Jobs can depend on each other (e.g. wait for `lint` before build/test)
- Runs tests; pipeline fails if tests fail

**Secrets:** Use GitHub repository secrets for environment variables in CI instead of committing `.env`.

### Step 3: Dockerfile (Node.js example)

- Base image e.g. Node.js 20 Alpine (small, secure)
- `WORKDIR` / create `/app` and set working directory
- `EXPOSE` the port your app listens on (e.g. 3000)
- Copy `package.json` / lockfile and install dependencies
- After build, prune dev dependencies for a smaller production image

### Choosing a deployment service

| Service | Notes |
|---------|--------|
| **Netlify** | Great for static frontends; built-in forms, serverless functions |
| **Railway** | One-click Postgres; Docker-friendly |
| **Fly.io** | Docker-based; fast edge deployment |
| **Heroku** | Simple UI; add-ons; **no free tier** (as of slides) |
| **AWS** | Very flexible; steeper learning curve; can get costly at scale |

**Workshop choice (example):** Frontend on **Vercel**, backend on **Render**.

- Vercel is optimised for frontends (e.g. Next.js, React), global CDN, automatic CI/CD from Git.
- Render: easy Dockerized backends, built-in Postgres, persistent services and background jobs; con: slow cold starts.

**Why not only Vercel for the API?** Vercel serverless functions have short timeouts, cold starts, and limited memory — not ideal for a long-running API or heavy DB logic.

### Step 4: Deploy backend (Render)

- Create a new Web Service
- **Non-Docker:** Render installs dependencies and runs your app from your build/start commands
- **Docker:** Render builds and runs from your Dockerfile
- Add environment variables from your local `.env` for the deployed app
- When steps succeed, the service is live

**AWS (alternative):** Serverless vs IaaS vs EC2 — as covered in slides.

### Step 5: Deploy frontend (Vercel)

- Use your **deployed backend URL** in the frontend before deploying.
- Select your framework (e.g. React) and root directory (important for monorepos).
- Optional: add a CD pipeline on Vercel — *slides note: only if you really want to; speaker said they personally wouldn't bother for simple cases.*

### Step 6: Custom domain (optional)

Configure DNS at your domain provider and in Vercel (or your host).

---

## Git practices

### Branching models *(slide diagrams)*

- Trunk-based branching
- Main–development branching
- GitFlow

### Branch protection

- You can define more complex rules; basic protection is often enough
- Merges go through pull requests
- *(Additional rule examples as in slides)*

### GitHub Issues

Linking PRs to issues: e.g. `Fixed #2` in a commit message closes the issue when the PR merges.

**Keywords:** close, closes, closed, fix, fixes, fixed, resolve, resolves, resolved

### Conventional Commits

Lightweight convention for clear history:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Common types:**

- **feat:** — New behaviour (new page, new button, etc.)
- **chore:** — Maintenance (merge commits, deps, project init)
- **fix:** — Small fixes and bugs (copy, typos, syntax, missing lines)

**Example:**

```
feat(auth): add login button on login page that redirects to dashboard.
```

**Another example (with body):**

```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

---

## Closing

**Time to deploy your app** — If you don't have your own project, use the workshop repos (frontend / backend).

Access the slides via the link shared in session.

**Thank you** — Please fill out the sign-up form if you haven't already.
