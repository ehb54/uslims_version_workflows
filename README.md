# Reusable GitHub Actions Workflows

This repository hosts reusable GitHub Actions workflows intended to be
called from other repositories via `workflow_call`.

## Workflows

### set-version-tag.yml

Reusable workflow that:
- Writes a provided version string to a `VERSION` file
- Optionally commits the file to `main`
- Optionally creates and pushes a tag `v<version>`

Inputs:
- `version` (string)
- `perform_release` (boolean)

`perform_release = false` → dry-run  
`perform_release = true`  → commit + tag

### release.yml (example)

Example dispatcher workflow showing how a repository can provide a manual UI
(`workflow_dispatch`) and call `set-version-tag.yml`.

This file is included as a reference and may be copied into other repositories.

## Notes

- This repository must remain public for reuse from private repos
- All operations target the `main` branch