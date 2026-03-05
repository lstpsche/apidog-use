---
name: apidog-configure
description: Configure the Apidog plugin for the current project. Creates .apidog.json
  config file, adds it to .gitignore, and guides through obtaining credentials.
---

# Configure Apidog Plugin

Set up the Apidog MCP server for the current project.

## Steps

### 1. Check for existing configuration

Look for `.apidog.json` in the project root. If it already exists, read it and ask the user if they want to reconfigure or update specific fields.

### 2. Gather credentials

Walk the user through each value they need:

**Access Token:**
- Go to [Apidog](https://apidog.com) > hover your avatar (top-right) > Account Settings > API Access Tokens
- Create a new token and copy it
- Ask the user to paste it

**Project ID:**
- Open the desired project in Apidog
- Go to Settings (left sidebar) > Basic Settings
- Copy the Project ID number
- Ask the user to paste it

**Modules:**
- Within the project, each module/collection has its own settings page with a Module ID
- Ask the user to list their module names and IDs as pairs (e.g. "backend = 1234, payments = 5678")
- If the user is unsure, tell them: open each module's settings in Apidog to find the ID

### 3. Create `.apidog.json`

Write the config file to the project root:

```json
{
  "accessToken": "<the token they provided>",
  "projectId": "<the project ID they provided>",
  "modules": {
    "<name>": <id>
  }
}
```

### 4. Add to `.gitignore`

Check if `.gitignore` exists in the project root:
- If it exists, check if `.apidog.json` is already listed. If not, append `.apidog.json` to it.
- If it doesn't exist, create `.gitignore` with `.apidog.json` as its first entry.

Explain to the user: "Added `.apidog.json` to `.gitignore` so your access token stays out of version control."

### 5. Verify the MCP server can connect

Call `apidog_modules` to verify the configuration works. If it succeeds, show the list of modules. If it fails, help the user troubleshoot (wrong token, wrong project ID, etc.).

### 6. Done

Tell the user their Apidog plugin is configured and ready. Briefly mention:
- They can use `apidog_list`, `apidog_get`, `apidog_export` to browse their API docs
- They can use the `apidog-lookup` skill to search endpoints
- They can use the `apidog-enrich` skill to improve documentation quality
- They can run `/analyze-api-docs` to audit their documentation coverage
