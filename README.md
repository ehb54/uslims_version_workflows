# GitHub Actions – Release Workflows

This repository provides reusable GitHub Actions workflows for creating
versioned release pull requests.

## Workflows

### `create-release-pr.yml`

Reusable workflow invoked via `workflow_call`.

**What it does**
- Creates (or reuses) a `release-v<version>` branch
- Writes the version to the `VERSION` file
- Commits the change
- Opens a pull request to `main`
- Fails if a tag `v<version>` already exists

**Inputs**
- `version` (string, required)  
  Examples: `1.2.3`, `1.2.3-RC1`

---

### `start-release.yml`

Manually triggered dispatcher workflow (`workflow_dispatch`).

**What it does**
- Prompts for a version number
- Calls `create-release-pr.yml`

This file is intended to be copied into consuming repositories to provide
a simple “Start Release” button in GitHub.

---

## Usage

From another repository:

```yaml
jobs:
  release:
    uses: ehb54/uslims_version_workflows/.github/workflows/create-release-pr.yml@full-sha
    with:
      version: String