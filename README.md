<div align="center">

# Flowlu MCP

<p>
  <a href="https://github.com/flowluhq/mcp_plugin_cursor"><img src="https://img.shields.io/badge/MCP-Compatible-blueviolet" alt="MCP Compatible"></a>
  <a href="https://www.flowlu.com/"><img src="https://img.shields.io/badge/Flowlu-Platform-green" alt="Flowlu"></a>
  <a href="https://cursor.com/"><img src="https://img.shields.io/badge/Cursor-Ready-orange" alt="Cursor Ready"></a>
</p>

**Connect AI agents to your work management platform — tasks, CRM, projects, and deals. Project management and work flows built to work together.**

</div>

## Overview

This Cursor plugin provides a pre-configured MCP (Model Context Protocol) connection to [Flowlu](https://www.flowlu.com/) — giving AI assistants like Cursor secure access to your tasks, CRM, projects, deals, and more through the hosted MCP server.

## What is Flowlu?

[Flowlu](https://www.flowlu.com/) is an all-in-one work management platform that connects project management, CRM, and business workflows. Teams use Flowlu to:

- Manage tasks with customizable statuses and assignments
- Track deals and leads in the CRM
- Run projects with stages, milestones, and collaboration
- Handle invoicing, finance, and time tracking
- Build knowledge bases and document workflows

## What's Inside

This plugin bundles:

| Component | Description |
|-----------|-------------|
| `.mcp.json` | Pre-configured MCP server pointing to Flowlu hosted service (`https://mcp.flowlu.com/mcp`) |
| `.cursor-plugin/plugin.json` | Plugin manifest with metadata and logo |
| `assets/logo.svg` | Flowlu branding for the Cursor marketplace |

## Quick Setup (Cursor)

### 1. Creating an API Key


1. Log in to your [Flowlu account](https://my.flowlu.com/).
2. Click your **profile picture** in the top-right corner.
3. Select **Portal Settings**.
4. Go to **For Developers > API**.
5. Click **Add** to create a new API key.
6. Give the key a name and assign the necessary module permissions (CRM, Tasks, Projects, etc.).
7. Copy the generated API Key. 

### 2. Add MCP server in Cursor

Open **File → Preferences → Cursor Settings → Tools & MCP** and add:

```json
{
  "mcpServers": {
    "flowlu": {
      "type": "http",
      "url": "https://mcp.flowlu.com/mcp",
      "headers": {
        "x-account-code": "<YOUR_COMPANY>",
        "Authorization": "Bearer <API_TOKEN_FROM_PORTAL>"
      }
    }
  }
}
```

Replace `YOUR_COMPANY` and `API_TOKEN_FROM_PORTAL` with your values.

You can also install this plugin from the Cursor Marketplace — it will add the MCP configuration automatically.

### 3. Test

In Cursor chat (Agent mode):

- *"Show my tasks"*
- *"Create task 'Prepare report'"*
- *"Show comments for task #42"*

## Available Tools

The Flowlu MCP server provides these tools:

| Tool | Description |
|------|-------------|
| `describe_modules` | Get map of modules and relations. Call first. Use `module` param for details. |
| `describe_entity` | Get full schema of entity fields (types, descriptions, FK). Use before create/update. |
| `list_records` | List records for any entity with search, filters, and pagination |
| `get_record` | Get a single record by ID |
| `create_record` | Create a new record (`confirm` — preview vs execute) |
| `update_record` | Update existing record (`confirm` — preview vs execute) |
| `list_comments` | Get comments for a record (task, deal, project, etc.) |
| `create_comment` | Add a comment to a record (`confirm` — preview vs execute) |
| `list_files` | List files attached to a record |
| `list_users` | List portal users (search and pagination) |

**Modules:** `agile`, `calendar`, `company`, `core`, `crm`, `customfields`, `fin`, `products`, `st`, `task`, `timetracker`

Recommended flow: call `describe_modules` first to discover structure, then `describe_entity` for the target entity before data operations.

## Example Prompts

| Prompt | Tools used |
|--------|------------|
| *"Show my tasks"* | `list_records` (module=`task`, entity=`tasks`) |
| *"Create task 'Prepare report'"* | `create_record` with `confirm` |
| *"Show comments for task #42"* | `list_comments` |
| *"List deals in pipeline"* | `list_records` (module=`crm`, entity=`lead`) |
| *"Add comment to project 10"* | `create_comment` with `confirm` |

## Security

- Write operations (create, update, add comment) require two-step confirmation — the tool returns a preview first; pass `confirm=true` only after user approval
- Rate limits: 20 writes per minute, 1000 per hour
- Some modules (finance, products, etc.) are read-only by default
- Never store tokens in public repositories or shared configs

## Configuration Reference

| Parameter | Header | Description |
|-----------|--------|-------------|
| Account code | `x-account-code` | Your Flowlu subdomain (e.g. `yourcompany` for `yourcompany.flowlu.com`) |
| MCP token | `Authorization: Bearer <token>` | Token from System Settings → API Settings in your portal |

## Community & Support

- [Flowlu Help Center](https://www.flowlu.com/help/)
- [Flowlu API Documentation](https://www.flowlu.com/api/)
- [GitHub Repository](https://github.com/flowluhq/mcp_plugin_cursor) — issues and contributions welcome

## Links

- [Flowlu](https://www.flowlu.com/) — platform homepage
- [Sign up for Flowlu](https://my.flowlu.com/register/)
- [Cursor Marketplace](https://cursor.com/marketplace)

## Contributing

Contributions are welcome. Please open an issue or submit a pull request on [GitHub](https://github.com/flowluhq/mcp_plugin_cursor).
