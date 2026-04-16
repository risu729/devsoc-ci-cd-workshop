---
layout: section
---

# Delivery workflows

## CI, previews, and safe release workflows

---
layout: quote
---

# Let AI move fast, not break prod

## AI can generate changes quickly. CI, preview deployments, and branch protection are what keep that speed safe.

---
layout: items
cols: 2
---

# CI & CD

::items::

<div>
  <h2>Continuous Integration</h2>
  <ul>
    <li>Keeps code quality consistent on every change</li>
    <li>Finds bugs, regressions, and vulnerabilities early</li>
    <li>Turns "works on my machine" into shared proof</li>
  </ul>
</div>

<div>
  <h2>Continuous Delivery / Deployment</h2>
  <ul>
    <li>Makes shipping changes safe and repeatable</li>
    <li>Delivers or deploys automatically after checks pass</li>
    <li>Reduces manual release mistakes and slow handoffs</li>
  </ul>
</div>

---
layout: items
cols: 2
---

# Harness engineering

leveraging Codex in an agent-first world

::items::

<div>
  <ul>
    <li>"Humans steer. Agents execute."</li>
    <li>A good companion piece on agent-first engineering, repository legibility, and feedback loops</li>
  </ul>
</div>

<div class="flex flex-col items-center justify-center">
  <QRCode
    :width="220"
    :height="220"
    type="svg"
    data="https://openai.com/index/harness-engineering/"
    :dotsOptions="{ type: 'extra-rounded', color: 'purple' }"
  />

  <div class="mt-3 text-lg text-center">
    openai.com/index/harness-engineering/
  </div>
</div>

---

# What belongs in a student pipeline?

1. Install dependencies
2. Lint
3. Format check
4. Type-check if the stack has one
5. Build
6. Run tests when the repo has meaningful ones

---
layout: items
cols: 2
---

# GitHub Actions

::items::

<div>
  <div class="provider-row">
    <img src="https://cdn.simpleicons.org/github/181717" alt="GitHub" class="provider-icon" />
    <strong>GitHub Actions</strong>
  </div>
  <ul>
    <li>Free on public repositories</li>
    <li>Private repos get `2000` minutes on Free and `3000` on Pro</li>
    <li>Same core idea as GitLab CI: YAML pipelines on repo events</li>
  </ul>
</div>

<div>
  <h2>Student recommendation</h2>
  <ul>
    <li>Join the GitHub Student Developer Pack</li>
    <li>It gives students free `GitHub Pro`</li>
    <li>It also includes `GitHub Copilot` for students</li>
    <li>That makes private-repo CI limits less painful</li>
  </ul>
</div>

---

# GitHub Actions syntax

- A workflow is one YAML file in `.github/workflows/`
- `jobs:` contains units of work, and `steps:` run top to bottom inside each job
- `uses:` pulls in a reusable action, while `run:` executes your own shell command

```yaml
# This file is one workflow.
name: CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  test:
    # One job running on one runner.
    runs-on: ubuntu-latest

    steps:
      - name: Check out the repository
        uses: actions/checkout@v4

      - name: Install dependencies
        run: bun install --frozen-lockfile

      - name: Run tests
        run: bun test
```

```yaml
# `uses:` reuses an action.
- uses: actions/checkout@v4

# `run:` executes a shell command.
- run: bun test
```

---

# Traditional staging vs preview deployments

- Traditional staging environment: one long-lived pre-production environment shared by everyone
- Preview deployment: one deployment per pull request
- Small student projects usually gain more from previews than from maintaining a full staging stack
- Staging becomes worth it later when multiple changes must be integrated together before release

---
layout: statement
---

# Preview deployments are the modern default

## Let reviewers click the change.

---
layout: items
cols: 2
---

# Actual Vercel / Cloudflare workshop

::items::

<div>
  <ul>
    <li>Example repo for the actual deployment workshop setup</li>
    <li>Shows a provider-agnostic frontend and backend split</li>
    <li>Use it for the real `Vercel` and `Cloudflare Workers` walkthrough</li>
  </ul>
</div>

<div class="flex flex-col items-center justify-center">
  <QRCode
    :width="220"
    :height="220"
    type="svg"
    data="https://github.com/devsoc-unsw/deployment-workshop-example"
    :dotsOptions="{ type: 'extra-rounded', color: 'purple' }"
  />

  <div class="mt-3 text-lg text-center">
    github.com/devsoc-unsw/deployment-workshop-example
  </div>
</div>

---

# Why GitHub Actions can be dangerous

- Workflow code runs with repository context
- Badly designed workflows can leak secrets or deploy untrusted code
- The common student mistake is letting outside pull requests trigger powerful jobs
- Safe rule: disable deployment or secret-using workflows for PRs from outside collaborators unless you fully understand the permissions model
