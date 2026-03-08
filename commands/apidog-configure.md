---
name: apidog-configure
description: Configure the Apidog plugin for the current project. Creates .apidog.json
  config file, adds it to .gitignore, and guides through obtaining credentials.
---

# Configure Apidog Plugin

Set up the Apidog MCP server for the current project. Supports single-project or multi-project configurations.

## Steps

### 1. Check for existing configuration

Look for `.apidog.json` in the project root. If it already exists, read it and ask the user if they want to:
- Reconfigure from scratch
- Add another project to the existing config
- Update specific fields

### 2. Gather credentials

Walk the user through each value they need:

**Access Token:**
- Go to [Apidog](https://apidog.com) > hover your avatar (top-right) > Account Settings > API Access Tokens
- Create a new token and copy it
- Ask the user to paste it
- Note: one token is shared across all projects in the config

**Projects (one or more):**

For each project the user wants to add:

- **Project name:** a short identifier (e.g. "main", "test", "staging"). Used to target the project in tool calls.
- **Project ID:** Open the project in Apidog > Settings (left sidebar) > Basic Settings > copy the Project ID number.
- **Modules:** Within the project, each module/collection has its own settings page with a Module ID. Ask for name/ID pairs (e.g. "backend = 1234, payments = 5678"). If unsure, tell them: open each module's settings in Apidog to find the ID.

After each project, ask: "Do you want to add another project?"

### 3. Create `.apidog.json`

Write the config file to the project root.

**Single project** (when only one project is configured):

```json
{
  "accessToken": "<the token they provided>",
  "projectId": "<the project ID>",
  "modules": {
    "<name>": <id>
  }
}
```

**Multiple projects:**

```json
{
  "accessToken": "<the token they provided>",
  "projects": [
    {
      "name": "<project name>",
      "projectId": "<project ID>",
      "modules": {
        "<name>": <id>
      }
    },
    {
      "name": "<project name>",
      "projectId": "<project ID>",
      "modules": {
        "<name>": <id>
      }
    }
  ]
}
```

Both formats are supported. The single-project format is automatically treated as one project named "default". Use the multi-project format when managing more than one Apidog project.

### 4. Add to `.gitignore`

Check if `.gitignore` exists in the project root:
- If it exists, check if `.apidog.json` is already listed. If not, append `.apidog.json` to it.
- If it doesn't exist, create `.gitignore` with `.apidog.json` as its first entry.

Explain to the user: "Added `.apidog.json` to `.gitignore` so your access token stays out of version control."

### 5. Verify the MCP server can connect

Call `apidog_modules` to verify the configuration works. If it succeeds, show the list of projects and their modules. If it fails, help the user troubleshoot (wrong token, wrong project ID, etc.).

For multi-project configs, verify each project individually by calling `apidog_modules` with the `project` parameter for each configured project name.

### 6. Done

Tell the user their Apidog plugin is configured and ready. Briefly mention:
- They can use `apidog_list`, `apidog_get`, `apidog_export` to browse their API docs
- They can use the `apidog-lookup` skill to search endpoints
- They can use the `apidog-enrich` skill to improve documentation quality
- They can run `/analyze-api-docs` to audit their documentation coverage
- For multi-project setups: pass `project: "<name>"` to target a specific project in tool calls

### Advanced: `APIDOG_CONFIG_PATH`

If `.apidog.json` is not in the project root or any parent directory, the user can set the `APIDOG_CONFIG_PATH` environment variable to an explicit file path. Add it to the MCP server env in `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "apidog": {
      "command": "npx",
      "args": ["-y", "@lstpsche/apidog-mcp"],
      "env": {
        "APIDOG_CONFIG_PATH": "/absolute/path/to/.apidog.json"
      }
    }
  }
}
```
