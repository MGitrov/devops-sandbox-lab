# DevOps Sandbox Lab

A "mini-production system" built to learn the full DevOps lifecycle end-to-end:

**Code → CI/CD Pipeline → Infrastructure as Code → Cloud/Container Deployment → Observability**

This is a personal learning project, not a production system. Every design decision
prioritizes **understanding** and **strict cost safety** over realism or scale.

## The application under test

**[Yelb](https://github.com/mreferre/yelb) (Cloned commit: 55ee1b8)** - a small open-source multi-tier demo app
(frontend + backend API + Redis + Postgres). Not written by me; used purely as a
realistic-enough workload to deploy, break, and observe.

## Architecture

```
GitHub repo (this repo)
    │
    ├─ GitHub Actions: lint → build → push image → tf plan/apply → deploy
    │
    ▼
AWS (budget-capped, tagged, auto-torn-down)
    VPC (public subnets only, no NAT Gateway)
    └─ EC2 t3.micro running k3s
         └─ Yelb (4 pods) + Prometheus + Grafana
```

## Phases

| Phase | Focus | Status |
|---|---|---|
| [1 - Containerization](docs/01-phase1-containers.md) | Docker, Docker Compose, local testing | 🟡 in progress |
| more to be added...|

## Repo structure

```
devops-lab/
├── README.md              ← you are here
├── docs/
│   ├── 01-phase1-containers.md   ← finished/polished explanation, per phase
│   └── journal.md         ← messy running log, written as I go
├── yelb/                  ← the demo app lives here (cloned/forked)
├── terraform/             ← IaC
└── .github/workflows/     ← CI/CD
```

## How the docs work

- **`journal.md`** - informal, dated notes-to-self. Written *during* the work.
  Confusion, half-finished thoughts, dead ends are all welcome here.
- **`docs/0X-*.md`** - the cleaned-up version, written *after* a phase is actually
  done. This is the "if I had to explain this to another junior, what would I say"
  version.

Journal first, polish later. Don't try to write the polished version while still learning it.
