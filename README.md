# GitHub Actions — Release Workflows

This repository provides reusable GitHub Actions workflows for managing versioned releases with automatic VERSION file validation.

## Workflows

### `create-bump-pr.yml`

Reusable workflow invoked via `workflow_call`.

**What it does:**
- Creates a `bump-to-v<version>` branch
- Updates the `VERSION` file with the specified version
- Commits and pushes the change
- Opens a pull request to `main`
- Fails if tag `v<version>` already exists

**Inputs:**
- `version` (string, required) — Examples: `1.2.3`, `1.2.3-RC1`

**Idempotent:** Safe to re-run; skips if VERSION already matches or PR exists.

---

### `bump-version.yml`

Manual dispatcher workflow (`workflow_dispatch`) for initiating releases.

**What it does:**
- Prompts for a version number via GitHub UI
- Calls `create-bump-pr.yml` with the provided version

Copy this file into consuming repositories to provide a "Bump Version" button.

---

## Usage

### In this repository
Click **Actions** → **Start Release** → **Run workflow** → enter version

### From another repository
```yaml
jobs:
  release:
    uses: ehb54/uslims_version_workflows/.github/workflows/create-bump-pr.yml@<commit-sha>
    with:
      version: "1.2.3"
```

---

## Bump Version Process

1. Run **Start Release** workflow with desired version (e.g., `1.2.3-RC1`)
2. Workflow creates PR with VERSION file updated
3. Review and merge PR to `main`