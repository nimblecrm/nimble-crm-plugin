# Nimble CRM plugin

Work with your [Nimble](https://www.nimble.com/) CRM from Claude, ChatGPT, and Codex. The plugin connects the AI
assistant to the Nimble MCP server and adds skills that teach it safe, repeatable CRM workflows: checking for duplicates
before saving a contact, discovering real field and stage IDs before updating a deal, and reading data before changing
it.

## Skills

| Skill | What it does |
| --- | --- |
| `find-contacts-in-nimble` | Find and filter people and companies, see who has not been contacted recently, and spot likely duplicates |
| `save-contact-to-nimble` | Create or update a person or company without creating duplicates, and merge duplicates after review |
| `search-deals-in-nimble` | Find, inspect, and compare deals by owner, pipeline, stage, status, dates, amounts, or custom fields |
| `manage-deals-in-nimble` | Create or update individual deals after discovering writable fields, pipelines, and stages |
| `read-messages-in-nimble` | Search and browse email, find a message by sender or subject, and summarize correspondence with a contact |
| `manage-contact-fields-in-nimble` | Create or update contact field tabs, groups, custom fields, and choice options |
| `remember-in-nimble` | Save durable Personal or Team AI Context that future Nimble AI workflows can use |

The assistant picks the right skill from your request, for example "Who among my clients haven't I contacted in 60
days?" or "Add Jane Doe from Acme to Nimble".

## Installation

### Claude and Cowork

Add **Nimble CRM** from the [Claude directory](https://claude.ai/directory), then sign in to Nimble when Claude asks you
to connect.

### Claude Code

```
/plugin marketplace add nimblecrm/nimble-crm-plugin
/plugin install nimble-crm@nimble-crm-plugin
```

Then run `/mcp`, select the **nimble** server, and authenticate with your Nimble account.

### ChatGPT and Codex

Add **Nimble CRM** from the ChatGPT and Codex plugin directory, then sign in to Nimble when asked to connect.

## Authentication and data

The plugin declares one remote MCP server, `https://app.nimble.com/mcp`, and authenticates with OAuth in your browser.
It contains no scripts, hooks, or local programs and sends data nowhere else. The assistant reads and changes only the
Nimble records your account can access, and asks before destructive changes such as merging contacts.

## Support

Contact Nimble support at [support.nimble.com](https://support.nimble.com/).
