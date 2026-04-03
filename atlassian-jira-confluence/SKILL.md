---
name: atlassian-jira-confluence
description: >
  Handles all tasks involving Jira and Confluence via the Atlassian Rovo MCP Server.
  Use this skill whenever the user wants to create or update Confluence pages, write
  Jira tickets, search issues, generate documentation from templates, or do anything
  involving Atlassian tools — even if they don't say "Confluence" or "Jira" explicitly.
  Trigger on phrases like "write a Confluence page", "create a Jira ticket", "document
  this in Confluence", "log source FAQ", "update the wiki", "add a Jira comment",
  "search Jira for...", or any request that implies structured knowledge-base or
  project-tracking work in an Atlassian environment.
---

# Atlassian Jira & Confluence Skill

This skill connects agent to your Atlassian workspace via the **Atlassian Rovo MCP Server**,
enabling you to create Confluence articles, manage Jira tickets, and search across both platforms
directly from your conversation.

---

## Confirmation Protocol (Always Apply)

Before calling **any MCP tool that writes, creates, updates, or deletes data**, you must:

1. **Show a preview** — Present the exact content or parameters you intend to send, formatted clearly for the user to read.
2. **Ask for explicit approval** — End with a clear confirmation prompt, e.g.:
   > "Does this look correct? Reply **yes** to proceed, or tell me what to change."
3. **Wait** — Do not call the MCP tool until the user replies with an affirmative.

If the user requests changes, update the preview and ask again. Only proceed once confirmed.

**Read-only operations** (search, read/fetch page content, list transitions) are exempt — execute those immediately without a confirmation step, since they have no side effects.

**Preview format by action type:**

| Action                 | What to show before confirming                             |
| ---------------------- | ---------------------------------------------------------- |
| Create Jira issue      | Project, issue type, summary, description, labels/priority |
| Update Jira issue      | Field name → current value → new value                     |
| Add Jira comment       | Ticket key + full comment text                             |
| Transition Jira status | Ticket key + current status → target status                |
| Create Confluence page | Space, parent page, title + full article content           |
| Update Confluence page | Title + diff or summary of what changes                    |

---

## MCP Configuration (First-Time Setup)

If the Atlassian Rovo MCP Server has not been connected yet, guide the user through the following steps.

> **Architecture note**: The Atlassian Rovo MCP Server is a **remote, cloud-hosted server** at
> `https://mcp.atlassian.com/v1/mcp`. There is nothing to install locally — you just point your
> MCP client at that URL and authenticate.

### Step 1 — Connect your MCP client

Most modern clients (Gemini CLI, Claude Code, VS Code, Cursor) support HTTP MCP natively.
Add the following to your MCP config file (e.g. `.mcp.json` in your project root, or the client's settings UI):

```json
{
  "mcpServers": {
    "atlassian-rovo-mcp": {
      "url": "https://mcp.atlassian.com/v1/mcp"
    }
  }
}
```

The first time you use it, a browser window will open for **OAuth 2.1 login** — sign in with your
Atlassian account. No token needed for this flow.

### Step 2 — Set default constants (First Time Only)

The first time you use this skill, guide the user to provide their Atlassian site URL, Jira Project Key, and Confluence Space Key. 
Once they provide these values, you **MUST write them into the configuration file** of skill located at:
`references/config.json`.

```json
{
  "cloudId": "https://yoursite.atlassian.net",
  "jiraProjectkey": "YOURPROJ",
  "confluenceSpaceId": "YOURSPACE"
}
```

After updating the file, explicitly confirm to the user that the configuration has been saved successfully.

For **all** subsequent operations, read the values from `references/config.json`. **Never call `getAccessibleAtlassianResources`**.

For **all** search operations, enforce:
- `maxResults: 10` (Jira JQL)
- `limit: 10` (Confluence CQL)

---

## Available Features

### Feature 1 — Confluence Log Source FAQ

**When to use**: Any time the user asks to document a log source, write a Log Source FAQ, create a "how to integrate X" page, or add a new log source to the knowledge base.

**Instructions**: Read the full reference before starting:
```
references/confluence-log-source-faq.md
```

**Template location**:
```
assets/log-source-faq-template.md
```

**Quick summary of the workflow**:
1. Ask clarifying questions to fill out the template (one group at a time — don't overwhelm the user).
2. If the user provides URLs, fetch and read them; use their content in the article and list them in the References section.
3. Fill in the template, marking any unknown sections as `[TODO — verify with team]`.
4. Publish to Confluence using the MCP, or present the content for manual copy-paste.

---

## General Jira Operations

For Jira tasks not covered by a dedicated reference, follow these principles:

- **Search**: Use JQL with `maxResults: 10`. Execute immediately; present results as a concise table (key, summary, status, assignee).
- **Create issue**: Draft the full issue (project, type, summary, description, labels, priority), show it as a preview, and wait for user confirmation per the [Confirmation Protocol](#confirmation-protocol-always-apply).
- **Update issue**: Show field name → current value → new value. Confirm before calling the update tool.
- **Add comment**: Display the full comment text with the ticket key. Confirm before posting.
- **Transition status**: Fetch and list available transitions first (read-only, no confirmation needed), then show the proposed transition and confirm before applying.

---

## General Confluence Operations

For Confluence tasks not covered by a dedicated reference, follow these principles:

- **Search**: Use CQL with `limit: 10`. Execute immediately; summarise matching pages with title, space, and URL.
- **Read page**: Fetch and render content immediately — no confirmation needed.
- **Create page**: Show the complete article content (space, parent page, title, body) as a preview and confirm per the [Confirmation Protocol](#confirmation-protocol-always-apply) before creating.
- **Update page**: Show a clear summary of what will change (or the full new content if short). Confirm before updating.

---

## Extending this Skill

This skill is designed to grow. To add a new feature:

1. Add a reference file in `references/` describing the workflow.
2. Add any templates or assets in `assets/`.
3. Add a new **Feature** section in this `SKILL.md` that points to the reference.

Keep each reference file focused on one task or domain so they stay readable and maintainable.
