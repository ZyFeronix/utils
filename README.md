# utils

Reusable GitHub Actions workflows shared by Voom, VoomTV and Woom-Toon.
Callers reference `@main`. Once a `v1` release tag exists, switch callers to `@v1` so changes here don't hit every repo at once.

| Workflow | Purpose | Key inputs |
|---|---|---|
| `branch-flow.yml` | Enforces work branch → `dev` → `beta` → `main` on PRs | none |
| `sync-gitlab.yml` | Mirrors `main`, `beta` and tags to GitLab | `gitlab-repo`; secret `GITLAB_ACCESS_TOKEN` |
| `docker-smoke.yml` | Builds amd64 + arm64 and runs the repo's `scripts/docker-smoke.sh` (+ optional compose validation) | `max-image-mb`, `packaging-paths`, `compose-profiles` |
| `docker-publish.yml` | Smoke-tests, then publishes the multi-arch image to GHCR | `max-image-mb`, `release-version` |

Smoke-script contract: `scripts/docker-smoke.sh <image> <platform>` exits non-zero on failure and honours
`MAX_IMAGE_MB`, `SMOKE_WAIT_TRIES` and `SMOKE_RESTARTS`.

Example caller:

```yaml
jobs:
  branch-flow:
    uses: ZyFeronix/utils/.github/workflows/branch-flow.yml@main
```

This repository is public: never put secrets in these files. Pass them from the caller.
