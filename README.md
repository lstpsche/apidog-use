# apidog-use

Cursor plugin for managing [Apidog](https://apidog.com) API documentation. Provides an MCP server with 22 tools, agent skills, coding rules, and commands for working with OpenAPI specs and endpoint cases directly from your IDE.

> **Note:** This is a community plugin, not affiliated with or endorsed by Apidog.

## Setup

### 1. Install the plugin

Install from the [Cursor Marketplace](https://cursor.com/marketplace) or add manually by cloning this repo.

### 2. Get your Apidog credentials

- **Access Token**: Apidog > Account Settings > API Access Tokens
- **Project ID**: Open your project > Settings > Basic Settings > Project ID
- **Module IDs**: Open each module's settings page to find its ID

### 3. Configure your project

Create `.apidog.json` in your project root (add it to `.gitignore`):

```json
{
  "accessToken": "adgp_your_token_here",
  "projectId": "1234567",
  "modules": {
    "backend": 1234,
    "payments": 5678
  }
}
```

The `modules` value maps human-friendly names to Apidog module IDs. You'll use these names when calling MCP tools.

**Alternative:** You can also use environment variables (`APIDOG_ACCESS_TOKEN`, `APIDOG_PROJECT_ID`, `APIDOG_MODULES`) — they take precedence over `.apidog.json`. See the [MCP server docs](https://github.com/lstpsche/apidog-mcp) for details.

## What's Included

### MCP Server (22 tools)

Powered by [`@lstpsche/apidog-mcp`](https://www.npmjs.com/package/@lstpsche/apidog-mcp):

| Category | Tools |
|---|---|
| **Read** | `apidog_modules`, `apidog_export`, `apidog_list`, `apidog_get`, `apidog_folders` |
| **Write** | `apidog_import_openapi`, `apidog_wipe`, `apidog_update`, `apidog_delete`, `apidog_pipeline` |
| **Cases** | `apidog_create_cases` |
| **Diff & Analysis** | `apidog_diff`, `apidog_analyze` |
| **Schemas** | `apidog_list_schemas`, `apidog_get_schema`, `apidog_update_schema`, `apidog_delete_schema` |
| **Bulk** | `apidog_bulk_update` |
| **Export** | `apidog_export_markdown`, `apidog_export_curl`, `apidog_export_postman` |
| **Testing** | `apidog_run_test` |

### Skills

| Skill | Description |
|---|---|
| **apidog-lookup** | Look up endpoints, schemas, and documentation structure from Apidog |
| **apidog-enrich** | Enrich API docs with summaries, descriptions, and schemas grounded in actual code |
| **apidog-sync** | Detect and resolve drift between codebase and Apidog documentation |

### Rules

| Rule | Description |
|---|---|
| **apidog-conventions** | Safe usage patterns for Apidog MCP tools |
| **openapi-best-practices** | OpenAPI 3.1 documentation quality standards |

### Commands

| Command | Description |
|---|---|
| **analyze-api-docs** | Run a full documentation quality audit on an Apidog module |

## Limitations

Some Apidog features are **not available** through this plugin due to Apidog's public API limitations:

- **No direct endpoint CRUD** — endpoints are managed via OpenAPI spec import, not individual REST calls.
- **No endpoint case CRUD** — cases are created via Postman collection import internally.
- **Auto-generated empty "Success" cases** — Apidog always creates these on import; the MCP server detects and replaces them automatically.
- **No folder/module management API** — folders are controlled via `x-apidog-folder` in imported specs.
- **No test scenario editing** — only execution of existing scenarios is supported (via `apidog-cli`).
- **No API for**: environment variables, mock server config, endpoint comments, or change history.

These are Apidog API limitations as of March 2026, not plugin limitations. Workarounds are implemented where possible.

## Multi-Module Architecture

A single MCP server instance manages multiple Apidog modules. Pass the module name (e.g. `"backend"`, `"payments"`) to each tool call — the server resolves it to the correct module ID from `APIDOG_MODULES`.

## License

MIT
