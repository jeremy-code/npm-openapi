# npm-registry-openapi

[![Sync NPM Registry OpenAPI Schema](https://github.com/jeremy-code/npm-openapi/actions/workflows/sync.yml/badge.svg)](https://github.com/jeremy-code/npm-openapi/actions/workflows/sync.yml)

A hosted mirror of the [NPM Registry OpenAPI schema](https://github.com/npm/api-documentation),
automatically synced via GitHub Actions.

The upstream project publishes its schema only as a blob download from a Redocly-generated
doc site. This repo makes the generated `openapi.yaml` / `openapi.json` directly accessible
as plain files in a git repository.

For [openapi.yaml](openapi.yaml) as a URL: https://raw.githubusercontent.com/jeremy-code/npm-openapi/refs/heads/main/openapi.yaml.

## Files

| File           | Description                                         |
| -------------- | --------------------------------------------------- |
| `openapi.yaml` | Merged OpenAPI 3.0 schema (YAML)                    |
| `openapi.json` | Same schema in JSON format                          |
| `.npm-api-sha` | SHA of the last upstream commit that touched `api/` |

## How it works

A scheduled GitHub Actions workflow:

1. Calls the GitHub API to get the latest commit SHA that touched `api/` in `npm/api-documentation`
2. Compares it to the SHA stored in `.npm-api-sha`
3. If there are new commits, clones the upstream repo, runs `npm run generate` (which invokes
   `openapi-merge-cli`), converts the output to JSON, and opens a pull request
   No rebuild happens if the `api/` directory hasn't changed, even if other files in the upstream
   repo were updated.
