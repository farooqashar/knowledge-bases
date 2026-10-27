---
title: CI/CD
tags: [devops, git]
updated: 2026-10-27
---

# CI/CD

**Continuous Integration** automatically builds and tests every change pushed to a shared repository. **Continuous Delivery/Deployment** automatically gets changes that pass those checks ready for release (delivery) or straight into production (deployment).

## How it works

1. You push a commit or open a pull request.
2. The CI service sees the event and starts a **pipeline** on a fresh machine (a **runner**).
3. The pipeline runs **jobs** in order or in parallel: install dependencies, lint, test, build.
4. Results show up on the pull request as passing or failing **checks**.
5. On merge to the main branch, a deploy job ships the build, often to a preview environment first.

## Key terms

- **Pipeline / workflow**: the whole sequence, defined in a file in the repo.
- **Job / step**: a unit of work and the commands inside it.
- **Runner**: the machine that executes jobs (hosted by the provider or self-hosted).
- **Artifact**: a file a job produces and passes on (a build, a test report).
- **Secrets**: credentials injected at run time, never committed to the repo.
- **Branch protection**: a rule that blocks merging until required checks pass.

## In practice

A minimal GitHub Actions workflow at `.github/workflows/ci.yml`:

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements.txt
      - run: pytest
```

Workflows can also run on a schedule (`on: schedule: - cron: "0 6 * * *"`), which makes CI a handy place for small recurring jobs.

## Common pitfalls

- Slow pipelines that people learn to ignore. Cache dependencies and run tests in parallel.
- **Flaky tests** that fail at random; they erode trust in every red check.
- Printing secrets to logs.

## Related

- [Containers](containers.md)
- [Cron and scheduled jobs](cron.md)
- [Webhooks](webhooks.md)
