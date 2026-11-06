# Journal

Informal, dated log. Write here *while* working — confusion, dead ends, and
half-formed thoughts are all fine. Clean summaries go into the numbered phase
docs once a phase is actually finished.

---

## 2026-08-05 — Repo setup

**What I did:**
- Created the `devops-lab` repo structure: README, docs/, terraform/, yelb/,
  .github/workflows/

**What I learned:**
- Work in GitHub's Codespace.
- The `.devcontainer` directory.
- The `-C` flag in Git, used to run a command as if git was started in `<path>` instead of the current working directory (instead of using `cd`).
- The `rev-parse` command, turns Git's nicknames into exact addresses (for example, `HEAD` turns into the commit I'm on right now).

**Confused about / to revisit:**
- _fill in_

---

## 2026-11-06 — Phase 1: reading the compose file, finding the volumes

**What I did:**
- Confirmed `docker run hello-world` works inside the codespace.
- Read `yelb/deployments/localtest/docker-compose.yaml` before running it and
  predicted ports and volumes.
- Pulled `postgres:10.21` and `redis:4.0.2` and inspected them for volumes.

**What I learned:**
- `ports: 8080:80` is `HOST:CONTAINER`, so the UI will be on 8080.
- Startup order from `depends_on`: `redis-server` + `yelb-db` (in parallel) →
  `yelb-appserver` → `yelb-ui`. `depends_on` only waits for the container to
  *start*, not for the app inside to be *ready*.
- A container gets a volume in two ways: declared in compose (`volumes:`), or
  declared in the image (`VOLUME` in a Dockerfile). The Yelb compose file
  declares none; the `postgres` and `redis` base images do.
- `{"/var/lib/postgresql/data":{}}`: key = path inside the container, `{}` =
  no extra settings. If nothing is mounted there, Docker creates an
  **anonymous volume** (random hash name).
- `docker image inspect` only looks at local images, it never pulls.
  `docker run` / `compose up` pull automatically. Hence
  `Error response from daemon: No such image: postgres:10.21`.
- `grep` uses regex, not shell globs: `*` = zero or more of the previous
  thing, `.` = any character. `grep` already matches substrings, so
  `grep -i volume` is enough. `-i` = ignore case. `grep` is line-based, so it
  only printed the `"Volumes": {` line; context flags (`-A`) show lines around it.
- `--format` uses Go templates: case-sensitive, and the path must follow the
  nesting: `docker image inspect --format '{{json .Config.Volumes}}' redis:4.0.2`.
  `map has no entry for key "volumes"` = wrong spelling or wrong level.
- Explore vs extract: browse/`grep` when I don't know the structure;
  `--format` (or `jq`) when I know the exact path, especially in scripts,
  because it doesn't depend on output layout.

**Confused about / to revisit:**
- What exactly happens to anonymous volumes on `docker compose down` vs
  `down -v`? (Test this in the break-it drill.)
- Learn `jq` properly at some point.
- `version: "2.1"` in the compose file is obsolete; check the warning.

---

<!-- Add new entries above this line, newest at the bottom or top — pick one
     convention and stick with it. -->
