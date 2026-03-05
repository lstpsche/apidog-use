---
description: Enrich Apidog API documentation with summaries, descriptions, response
  schemas, and examples grounded in actual codebase logic. Use when the user wants
  to improve API docs quality, fill in missing fields, or add endpoint details.
alwaysApply: false
---

# Apidog Enrich

Improve API documentation in Apidog by adding missing summaries, descriptions, operation IDs, response schemas, and examples — always grounded in the actual codebase.

## When to Use

- User wants to improve API documentation quality
- User asks to add missing summaries, descriptions, or examples
- User wants to fill gaps in endpoint documentation
- After running `apidog_analyze` and finding low coverage

## Critical Rule

**Never invent or hallucinate endpoint descriptions, business logic, or response schemas.** Every piece of documentation must be grounded in the actual source code:

1. Read the controller/route file to understand what the endpoint does
2. Read the service/model code to understand the business logic
3. Read serializers/views to understand the response shape
4. Only then write the summary/description based on verified behavior

If you cannot find the source code for an endpoint, say so explicitly rather than guessing.

## Procedure

### 1. Assess current state

Export the current spec and run coverage analysis:

```
apidog_export(module: "<module>")
apidog_analyze(module: "<module>", checks: ["coverage"])
```

Review the coverage report to identify endpoints missing summaries, descriptions, operation IDs, or response schemas.

### 2. Read the codebase

For each endpoint that needs enrichment:

1. Find the route definition (e.g., `config/routes.rb` or route files)
2. Read the controller action to understand request handling
3. Read any service objects or model methods called by the action
4. Read serializers or response builders to understand the response shape
5. Note any authentication, authorization, or validation requirements

### 3. Write grounded documentation

Based on the code reading:

- **Summary**: One-line description of what the endpoint does (business perspective)
- **Description**: Detailed explanation including auth requirements, notable behavior, side effects
- **Parameters**: Document path params, query params, and request body with types and constraints
- **Responses**: Document success and error responses with accurate schemas

### 4. Apply updates

For individual endpoints, use `apidog_update`. For batch updates of summaries/tags/status, use `apidog_bulk_update`.

### 5. Verify

Re-run `apidog_analyze` to confirm coverage improved. Spot-check a few endpoints with `apidog_get` to verify the updates look correct.
