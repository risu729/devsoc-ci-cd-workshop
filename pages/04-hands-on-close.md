---
layout: section
---

# Hands-on and close

## Domains, Docker in context, and one realistic shipping workflow

---

# Domains and DNS

- A domain is the public address for your project
- DNS tells browsers where that project lives
- A custom domain makes a student project feel much more real
- Good workshop goal: leave with something reachable at a real URL

---
layout: items
cols: 2
---

# Domain recommendation

::items::

<div>
  <div class="provider-row">
    <img src="https://cdn.simpleicons.org/cloudflare/F38020" alt="Cloudflare" class="provider-icon" />
    <strong>Cloudflare Registrar</strong>
  </div>
  <ul>
    <li>Charges at-cost with no extra registrar markup</li>
    <li>Easy DNS management</li>
    <li>Good default for first-time domain buyers</li>
  </ul>
</div>

<div>
  <h2>The trade-off</h2>
  <ul>
    <li>Your domain uses `Cloudflare DNS`</li>
    <li>That is fine for most student projects</li>
    <li>It connects cleanly to `Vercel` and `Cloudflare Workers`</li>
  </ul>
</div>

---

# Docker

- Docker packages an app and its runtime into a portable container
- It helps teams run the same app locally, in CI, and in production
- Good fit for local databases, reproducible environments, and container-based hosts
- For most student projects, it is useful context, not the default deployment answer

---

# Docker in practice


<div class="provider-row">
  <img src="https://cdn.simpleicons.org/docker/2496ED" alt="Docker" class="provider-icon" />
  <strong>What students should remember</strong>
</div>

- A `Dockerfile` is a recipe for building an image
- An image is a packaged app snapshot
- A container is a running instance of that image
- The big idea is consistency across environments


```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json bun.lock ./
RUN bun install --frozen-lockfile
COPY . .
RUN bun run build
CMD ["bun", "run", "start"]
```

---

# When Docker is worth it

- When your app depends on specific runtimes or system packages
- When the deployment target expects a container image
- When you want the same setup in local dev, CI, and production
- When you need to run supporting services like Postgres or Redis locally
- Less important when a platform already handles build and runtime for you

---

# Portability and lock-in

- Managed platforms reduce ops work, but they can increase platform coupling
- Docker can improve portability because many providers can run the same container image
- Kubernetes helps run containers at scale, but it is not a beginner deployment default
- Self-hosted Kubernetes trades vendor lock-in for much higher ops responsibility
- Good principle: portability matters, but simplicity matters more for most student projects

---
layout: intro
class: text-center
---

# Q&A / project clinic

Bring one real idea and decide:

- `Vercel` or `Cloudflare Workers`?
- `Neon`, `Supabase`, `Firestore`, or `MongoDB Atlas`?
- Serverless or always-on backend?
