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

Store config in the environment.

### IV. Backing Services

Treat backing services as attached resources.

### V. Build, Release, Run

Strictly separate build and run stages.

### VI. Processes

Execute the app as one or more stateless processes.

### VII. Port Binding

Export services via port binding.

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

