# apidog-use

Cursor plugin for managing [Apidog](https://apidog.com) API documentation. Provides an MCP server with 22 tools, agent skills, coding rules, and commands for working with OpenAPI specs and endpoint cases directly from your IDE.

> **Note:** This is a community plugin, not affiliated with or endorsed by Apidog.

## Setup

### 1. Get your Apidog credentials

- **Access Token**: Apidog > Account Settings > API Access Tokens
- **Project ID**: Open your project > Settings > Basic Settings > Project ID
- **Module IDs**: Open each module's settings page to find its ID

### 2. Set environment variables

Add these to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.):

```bash
export APIDOG_ACCESS_TOKEN="your-token-here"
export APIDOG_PROJECT_ID="your-project-id"
export APIDOG_MODULES='{"backend":1234,"payments":5678}'
```

The `APIDOG_MODULES` value is a JSON map of human-friendly names to Apidog module IDs. You'll use these names when calling MCP tools.

### 3. Install the plugin

Install from the [Cursor Marketplace](https://cursor.com/marketplace) or add manually by cloning this repo.

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

## Multi-Module Architecture

A single MCP server instance manages multiple Apidog modules. Pass the module name (e.g. `"backend"`, `"payments"`) to each tool call — the server resolves it to the correct module ID from `APIDOG_MODULES`.

## License

[MIT](LICENSE)
