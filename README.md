# .github

Shared GitHub setup for every fethca repo.

## Issue templates

`.github/ISSUE_TEMPLATE` is used by every repo that doesn't define its own.

## Reusable workflows

Packages publish to npm with trusted publishing on a `v*.*.*` tag:

```yaml
on:
  push:
    tags: ['v*.*.*']

permissions:
  contents: read
  id-token: write

jobs:
  publish:
    uses: fethca/.github/.github/workflows/publish-package.yml@main
```

Apps lint, test, build their Docker image and redeploy every `docker-compose` service named after the repo:

```yaml
on:
  push:
    tags: ['v*.*.*']

jobs:
  deploy:
    uses: fethca/.github/.github/workflows/deploy-app.yml@main
    secrets: inherit
```

Pass `with: { test: false }` for apps without unit tests.

## Renovate

`default.json` is the shared Renovate preset: weekly grouped minor/patch updates, separate major updates, security fixes right away, and releases held 3 days before they are proposed. Each repo opts in with a `renovate.json`:

```json
{ "extends": ["github>fethca/.github"] }
```
