# GitHub Actions — Release Workflows

This repository provides reusable GitHub Actions workflows for managing versioned releases with automatic VERSION file validation.

## Workflows

### `create-release-pr.yml`

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

### `start-release.yml`

Manual dispatcher workflow (`workflow_dispatch`) for initiating releases.

**What it does:**
- Prompts for a version number via GitHub UI
- Calls `create-release-pr.yml` with the provided version

Copy this file into consuming repositories to provide a "Start Release" button.

---

### `verify-version-tag.yml`

Automatic validation workflow triggered on tag push.

**What it does:**
- Runs when any `v*` tag is pushed
- Validates tag format: `vX.Y.Z` or `vX.Y.Z-suffix`
- Verifies the `VERSION` file at the tagged commit exactly matches the tag version (without `v` prefix)
- Fails if there's a mismatch, preventing invalid releases

**Purpose:** Ensures VERSION file hygiene before releases proceed.

---

## Usage

### In this repository
Click **Actions** → **Start Release** → **Run workflow** → enter version

### From another repository
```yaml
jobs:
  release:
    uses: ehb54/uslims_version_workflows/.github/workflows/create-release-pr.yml@<commit-sha>
    with:
      version: "1.2.3"
```

---

## Release Process

1. Run **Start Release** workflow with desired version (e.g., `1.2.3-RC1`)
2. Workflow creates PR with VERSION file updated
3. Review and merge PR to `main`
4. Tag `main`
5. **verify-version-tag** workflow validates VERSION matches tag
6. Proceed if validation passes