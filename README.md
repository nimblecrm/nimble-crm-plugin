# Nimble CRM plugin

Work with your [Nimble](https://www.nimble.com/) CRM from Claude, ChatGPT, and Codex. The plugin connects the AI
assistant to the Nimble MCP server and adds skills that teach it safe, repeatable CRM workflows: checking for duplicates
before saving a contact, discovering real field and stage IDs before updating a deal, and reading data before changing
it.

## Skills

| Skill | What it does |
| --- | --- |
| `log-meeting-in-nimble` | Log a meeting from a transcript, notetaker recap, or summary: note, tasks, reminders, next meeting, deal stage, and a draft email, after one confirmation |
| `prepare-for-meeting-in-nimble` | Brief you before a meeting: who they are, the last exchange, promises on both sides, and what is still open |
| `find-contacts-in-nimble` | Find and filter people and companies, see who has not been contacted recently or is due a Stay in Touch reminder, spot likely duplicates, and save a search |
| `save-contact-to-nimble` | Create, update, tag, or remove a person or company without creating duplicates, set Stay in Touch reminders, and merge duplicates after review |
| `search-deals-in-nimble` | Find, inspect, and compare deals by owner, pipeline, stage, status, dates, amounts, or custom fields |
| `manage-deals-in-nimble` | Create or update individual deals after discovering writable fields, pipelines, and stages |
| `read-messages-in-nimble` | Search and browse email, find a message by sender or subject, and summarize correspondence with a contact |
| `manage-contact-fields-in-nimble` | Create or update contact field tabs, groups, custom fields, and choice options |
| `remember-in-nimble` | Save durable Personal or Team AI Context that future Nimble AI workflows can use |
| `get-help-with-nimble` | Answer how-to and troubleshooting questions from the Nimble Help Center |

The assistant picks the right skill from your request, for example "Who among my clients haven't I contacted in 60
days?" or "Add Jane Doe from Acme to Nimble".

The skills come with this plugin. Adding only the Nimble connector gives the assistant the same Nimble tools without
these skills.

## Installation

### Claude and Cowork

Install the **Nimble CRM** plugin from the [Claude directory](https://claude.ai/directory), then sign in to Nimble when
Claude asks you to connect. If you already added Nimble as a custom connector from
[app.nimble.com/mcp](https://app.nimble.com/mcp), the plugin uses the same server.

### Claude Code

```
/plugin marketplace add nimblecrm/nimble-crm-plugin
/plugin install nimble-crm@nimble-crm-plugin
```

Then run `/mcp`, select the **nimble** server, and authenticate with your Nimble account.

### ChatGPT and Codex

Install the **Nimble CRM** plugin from the ChatGPT and Codex plugin directory, then sign in to Nimble when asked to
connect. To connect Nimble without the plugin, follow the setup steps at
[app.nimble.com/mcp](https://app.nimble.com/mcp).

## Authentication and data

The plugin declares one remote MCP server, `https://app.nimble.com/mcp`, and authenticates with OAuth in your browser.
It contains no scripts, hooks, or local programs and sends data nowhere else. The assistant reads and changes only the
Nimble records your account can access, and asks before destructive changes such as merging or deleting contacts.

## Support

Contact Nimble support at [support.nimble.com](https://support.nimble.com/).
