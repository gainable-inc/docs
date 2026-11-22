# Gainable Documentation

This repository contains the documentation for [Gainable](https://build.gainable.dev), the AI-powered no-code app builder.

## Documentation Structure

The site has four tabs, one per reader: **Build**, **Manage**, **Use** and **Develop**. `docs.json` maps each page to its tab.

```
docs/
├── introduction.mdx          # Platform overview and the four paths
├── quickstart.mdx            # First app in 5 minutes
├── build-from-data.mdx       # Build from spreadsheets and other data
│
├── build/
│   ├── datasets.mdx          # Datasets, connections, sync and two-way sync
│   └── spreadsheets.mdx      # How Gaia reads spreadsheets
│
├── manage/
│   ├── overview.mdx          # Owning an app: where everything is
│   ├── who-does-what.mdx     # Every kind of user side by side
│   ├── access.mdx            # Access, roles, groups and rules
│   ├── data.mdx              # The app's Data page: sync, approval, two-way, schedule
│   ├── activity.mdx          # Activity log
│   ├── assistant.mdx         # Set up the Assistant: scheduled tasks, memory, tools
│   └── share-assistant.mdx   # Share the Assistant over MCP
│
├── use/
│   ├── overview.mdx          # Using Gainable: where to start
│   ├── apps-grid.mdx         # The Apps grid: tabs, account views, groups, widgets
│   ├── working-in-an-app.mdx # Pages, records, comments, live updates
│   ├── assistant.mdx         # Work with the Assistant
│   ├── ai-tools.mdx          # An app's Assistant in Claude, ChatGPT and other AI tools
│   ├── notifications.mdx     # Notifications, Messenger, team updates
│   └── mobile.mdx            # Home screen and push notifications
│
├── concepts/
│   ├── how-it-works.mdx      # How Gainable builds apps
│   ├── agents.mdx            # The build agent system
│   └── skills.mdx            # Built-in capabilities
│
├── prompting/
│   ├── overview.mdx          # Prompt strategy fundamentals
│   ├── best-practices.mdx    # Writing effective prompts
│   ├── examples.mdx          # Real prompt examples
│   └── common-patterns.mdx   # Templates for common apps
│
├── building/
│   ├── data-models.mdx       # Data and schemas
│   ├── views.mdx             # UI and pages
│   ├── real-time.mdx         # Live updates
│   └── collaboration.mdx     # Chat, files, comments
│
├── skills/
│   ├── chartjs.mdx           # Data visualization
│   ├── kanban.mdx            # Drag-drop boards
│   ├── weavy.mdx             # Collaboration features
│   └── design.mdx            # Themes and styling
│
├── mcp/                      # The Gaia MCP connector
│   ├── overview.mdx          # What it is, clients, scopes
│   ├── claude-code.mdx       # Setup in Claude Code
│   ├── claude.mdx            # Setup in Claude chat, Cowork, Desktop
│   ├── codex.mdx             # Setup in OpenAI Codex
│   ├── other-clients.mdx     # ChatGPT, Cursor, Gemini, Goose and others
│   ├── plugins.mdx           # Plugins for Claude Code, Cowork, Codex
│   ├── api-keys.mdx          # Account API keys for scripts, CI and headless clients
│   ├── tools.mdx             # Every tool and parameter
│   └── datasets.mdx          # Bring any source: your AI tool as the connector
│
└── reference/
    ├── api.mdx               # Generated app APIs
    └── troubleshooting.mdx   # Common issues
```

## Local Development

1. Install the Mintlify CLI:
   ```bash
   npm i -g mint
   ```

2. Run the local development server:
   ```bash
   mint dev
   ```

3. Open `http://localhost:3000` to preview the docs.

4. Before opening a pull request, run `mint broken-links` and `mint validate`.

## Deployment

Documentation is automatically deployed when changes are pushed to the main branch. Every pull request gets a Mintlify preview link.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full workflow: setup, fixing an item with Claude Code (`/docfix`), screenshots, and review rules.

## Resources

- [Mintlify Documentation](https://mintlify.com/docs)
- [Gainable App](https://build.gainable.dev)
