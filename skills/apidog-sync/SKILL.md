---
description: Sync Apidog API documentation with the actual codebase. Use when the
  user wants to detect drift between code and docs, find undocumented endpoints,
  or update docs after code changes.
alwaysApply: false
---

# Apidog Sync

Detect and resolve drift between Apidog API documentation and the actual codebase routes and controllers.

## When to Use

- User wants to check if API docs are up to date with the code
- User added new endpoints and wants to document them in Apidog
- User changed endpoint behavior and wants docs to reflect it
- User wants to find undocumented or removed endpoints

## Procedure

### 1. Export current Apidog state

```
apidog_export(module: "<module>")
```

Save or note the exported spec for comparison.

### 2. Build a spec from the codebase

Scan the codebase to build a picture of actual API endpoints:

1. Read route files (`config/routes.rb`, `config/routes/*.rb`, or framework equivalent)
2. For each route, read the controller action
3. Note the HTTP method, path, parameters, and response shape
4. Build or write a local OpenAPI spec file reflecting the code reality

### 3. Diff against Apidog

Use `apidog_diff` to compare the local spec against what Apidog currently has:

```
apidog_diff(module: "<module>", specPath: "<path-to-local-spec>")
```

This reports:
- Endpoints in code but missing from Apidog (undocumented)
- Endpoints in Apidog but missing from code (stale/removed)
- Endpoints present in both but with differences (drifted)

### 4. Review and confirm

Present the diff results to the user. For each discrepancy, explain:
- What changed and why it matters
- Whether it's a new endpoint, removed endpoint, or updated behavior

Get user confirmation before applying changes.

### 5. Apply updates

Based on confirmed changes:
- **New endpoints**: Use `apidog_import_openapi` or `apidog_update` to add them
- **Removed endpoints**: Use `apidog_delete` to clean up stale docs
- **Changed endpoints**: Use `apidog_update` to refresh their definitions
- **Bulk metadata**: Use `apidog_bulk_update` for tags, status, summaries

### 6. Verify

Re-run `apidog_diff` to confirm Apidog and codebase are now in sync. Report remaining discrepancies if any.
