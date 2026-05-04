# 12 Factor App

**Status:** 🔲 Not started
**Started:** —
**Completed:** —

---

## Why 12 Factor?

Born from Heroku's experience deploying hundreds of thousands of apps, it codifies best practices for SaaS to solve recurring problems: painful onboarding, environment drift, poor scalability, and software erosion over time.

**Core goals:** portability, deployability, scalability — without rewriting your app.

---

## Factors

### I. Codebase

One codebase tracked in revision control, many deploys. Git is used for this reason.

### II. Dependencies

Explicitly declare (requirements.txt, package.json) and isolate dependencies (venv, docker).

### III. Config

Store config in the environment. The same codebase deploys to dev, staging, and prod — only the env vars differ (e.g. `HOST`, `PORT` in a `.env` file pointing to environment-specific backing services).

### IV. Backing Services

Treat backing services as attached resources (e.g. S3, SMTP, Redis).

### V. Build, Release, Run

Strictly separate build and run stages:
1. **Build** — compile code into an executable artifact (e.g. `docker build` → image)
2. **Release** — combine artifact with config (e.g. image + `.env`) → versioned release (v1, v2, v3 or timestamped)
3. **Run** — execute the release in the target environment

### VI. Processes

Execute the app as one or more stateless processes. Twelve-factor processes are stateless and share-nothing. Any state must be stored in a backing service such as a DB or Redis cache. Sticky sessions are a violation of twelve-factor and should never be used or relied upon.

### VII. Port Binding

Export services via port binding. The app is self-contained and binds to a port itself (e.g. Flask on port 5000) rather than relying on an external web server like Apache. This makes it easy to become a backing service for another app.

### VIII. Concurrency

Scale out via the process model. Apps should scale horizontally (more processes), not vertically (bigger machines).

### IX. Disposability

Maximize robustness with fast startup and graceful shutdown.

### X. Dev/Prod Parity

Keep development, staging, and production as similar as possible.

### XI. Logs

Treat logs as event streams.

### XII. Admin Processes

Run admin/management tasks as one-off processes.

---

## Key Takeaways


## Questions / Things to revisit

