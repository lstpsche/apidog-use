---
description: Look up API endpoints, schemas, and documentation structure from Apidog.
  Use when the user asks about API endpoints, available routes, request/response
  formats, or wants to browse API documentation.
alwaysApply: false
---

# Apidog Lookup

Fetch live API documentation from Apidog before answering questions about endpoints, schemas, or API structure.

## When to Use

- User asks about available API endpoints or routes
- User wants to know the request/response format for an endpoint
- User asks about API documentation structure, folders, or organization
- User needs to find an endpoint by name, path, method, or tag
- User asks about component schemas or data models

## Procedure

### 1. Identify the module

Call `apidog_modules` to list available modules. If the user didn't specify a module, ask which one they mean or infer from context.

### 2. Find the endpoint(s)

Use the appropriate tool based on what the user needs:

- **Browse structure**: `apidog_folders` to see the folder tree and endpoint counts
- **Search by name/path/tag**: `apidog_list` with `query`, `folder`, `method`, or `tag` filters
- **Get full details**: `apidog_get` with the endpoint's `method` and `path` to retrieve parameters, request body, responses, and examples

### 3. For schemas

- `apidog_list_schemas` to browse available component schemas
- `apidog_get_schema` to get a specific schema definition

### 4. For full spec

If the user needs the complete OpenAPI spec or a broad overview, use `apidog_export` to get the full spec for the module.

### 5. Present results

Summarize the findings clearly. Include:
- Endpoint path and method
- Summary and description
- Key parameters and request body fields
- Response status codes and shapes
- Links to related endpoints if relevant
