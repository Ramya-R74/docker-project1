# Docker Mastery Project
### A Progressive Journey from Beginner to Advanced Docker Usage

![Docker Mastery Overview](images/docker-intro.png)

This repository contains one FastAPI application, packaged and delivered three different ways across three phases — beginner, intermediate, and advanced — so you can teach or demo the full maturity curve of real-world Docker + CI/CD practice.

The application code barely changes between phases; what changes is **how it's containerized, optimized, automated, and shipped.**

---

## Folder Structure
```
docker-mastery-project/
├── phase1-beginner/          # Ubuntu base, single-stage, manual Docker CLI
├── phase2-intermediate/      # Slim base, multi-stage, basic CI pipeline
├── phase3-advanced/          # Alpine base, cached CI/CD, scanning, push to Docker Hub + AWS ECR
├── DOCKER-INSTALLATION.md    # Docker installation guide
└── README.md                 # you are here
```

Each phase folder is self-contained (its own `Dockerfile`, `README.md`, requirements, and — from Phase 2 onward — its own GitHub Actions workflow), so you can `cd` into any one and follow along independently.

---

## Quick Start — Build & Run Each Phase
```bash
# Phase 1 — Beginner
cd phase1-beginner
docker build -t docker-mastery:phase1 .
docker run -d --name phase1-app -p 8000:8000 docker-mastery:phase1

# Phase 2 — Intermediate
cd ../phase2-intermediate
docker build -t docker-mastery:phase2 .
docker run -d --name phase2-app -p 8001:8000 docker-mastery:phase2

# Phase 3 — Advanced
cd ../phase3-advanced
docker build -t docker-mastery:phase3 .
docker run -d --name phase3-app -p 8002:8000 docker-mastery:phase3
```

## Compare Image Sizes Side by Side
```bash
docker images | grep docker-mastery
```

---

## Phase-wise Summary

| | Phase 1 — Beginner | Phase 2 — Intermediate | Phase 3 — Advanced |
|---|---|---|---|
| **Base image** | `ubuntu:22.04` | `python:3.12-slim` | `python:3.12-alpine` |
| **Build strategy** | Single-stage | Multi-stage | Multi-stage, non-root, `HEALTHCHECK` |
| **Automation** | None (manual CLI) | Basic CI (lint → test → build) | Full CI/CD (cache → scan → tag → push) |
| **Registries** | None | None | Docker Hub **and** AWS ECR |
| **Security** | n/a | n/a | Trivy scanning, GitHub Secrets, OIDC AWS auth |
| **Tagging** | n/a | n/a | commit SHA, `latest`, semantic version |
| **Approx. image size** | 400–600MB+ | ~150–180MB | ~80–120MB |

---

## Why This Structure Works
- The same app runs throughout, so the learning stays focused on Docker/DevOps, not application logic.
- Each phase fixes a concrete limitation of the one before it (image size → automation → production readiness).
- It mirrors how real teams mature their Docker usage over time.
- Every phase has its own README with copy-pasteable commands and a clear before/after story, ready to present.

---

## Suggested Demo Flow
1. Show Phase 1 running, then run `docker images` to show its size.
2. Show Phase 2's Dockerfile side-by-side with Phase 1's, rebuild, and compare size + build time.
3. Push a commit and show the Phase 2 GitHub Actions run (lint/test/build).
4. Show Phase 3's pipeline run end-to-end: cache hit on rebuild, Trivy scan output, and the final image tags appearing in both Docker Hub and ECR.
