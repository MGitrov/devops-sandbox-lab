# Phase 1 — Local Setup & Containerization

> Status: 🟡 in progress
> This file gets filled in properly once the phase is done. Use `journal.md` for
> day-to-day notes; come back here to write the clean summary.

## Goals

- Understand image layers, build context, and layer-order caching
- Understand container-to-container networking without an orchestrator
  (Docker Compose's DNS-based service discovery)
- Learn to distinguish stateless containers from the one thing that actually
  needs a persisted volume

## Checklist

- [x] Docker installed on Mac
- [ ] `docker run hello-world` confirmed working
- [ ] Yelb repo forked/cloned locally
- [ ] `docker-compose up` running Yelb locally, UI reachable in browser
- [ ] Confirmed votes persist across `yelb-appserver` restart
- [ ] Confirmed votes do NOT persist after `docker-compose down -v`
- [ ] Rebuilt at least one Dockerfile from scratch (multi-stage build)
- [ ] Added `.dockerignore` per service
- [ ] Tagged images explicitly (no `latest`)
- [ ] Pushed images manually to Docker Hub once
- [ ] Added basic `HEALTHCHECK` to each service
- [ ] (Stretch) Converted compose file to K8s manifests via `kompose`, deployed to Minikube

## Key concepts (fill in as understood)

### Image vs container
_TODO_

### Why layer order matters for caching
_TODO_

### How Compose service discovery works (and how it differs from Kubernetes Services)
_TODO_

### Why yelb-db needs a volume and the others don't
_TODO_

## Pitfalls hit (or avoided)

_Log anything here that bit you, even if you caught it before it became a problem._

## Open questions

_Things you're still fuzzy on going into Phase 2._
