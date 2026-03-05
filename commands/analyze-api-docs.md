---
name: analyze-api-docs
description: Run a full API documentation quality check on an Apidog module, reporting
  coverage gaps and validation issues.
---

# Analyze API Docs

Run a comprehensive documentation quality audit on an Apidog module.

## Steps

1. Ask the user which module to analyze (or use `apidog_modules` to list options).

2. Export the current spec:
   ```
   apidog_export(module: "<module>")
   ```

3. Run both coverage and validation checks:
   ```
   apidog_analyze(module: "<module>", checks: ["coverage", "validation"])
   ```

4. Present a structured report:

   **Coverage** — for each category (summary, description, operationId, response schema, examples):
   - Percentage of endpoints covered
   - List of endpoints missing that field

   **Validation** — issues found:
   - Endpoints with empty or placeholder text
   - Orphaned schemas (defined but never referenced)
   - Missing error response definitions
   - Inconsistent naming patterns

5. Suggest concrete next steps:
   - Which endpoints to prioritize (high-traffic or user-facing first)
   - Whether to use `apidog_enrich` skill for bulk enrichment
   - Whether to use `apidog_bulk_update` for quick metadata fixes
