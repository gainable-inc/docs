# Gainable Documentation

This repository contains the documentation for [Gainable](https://build.gainable.dev), the AI-powered no-code app builder.

## Documentation Structure

```
docs/
├── introduction.mdx          # Platform overview
├── quickstart.mdx            # First app in 5 minutes
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
│   ├── data-connectors.mdx   # Datasets and external sources
│   ├── data-models.mdx       # Data and schemas
│   ├── views.mdx             # UI and pages
│   ├── real-time.mdx         # Live updates
│   └── collaboration.mdx     # Chat, files, comments
│
├── copilots/                 # Gaia Copilot: conversational AI in apps
│   ├── overview.mdx
│   ├── configuring.mdx
│   ├── data-access.mdx
│   ├── knowledge.mdx
│   └── embedding.mdx
│
├── autopilot/                # Gaia Autopilot: autonomous, draft-and-approve
│   ├── overview.mdx
│   ├── objectives.mdx
│   ├── playbooks.mdx
│   ├── triggers.mdx
│   ├── tools.mdx
│   ├── risk-tiers.mdx
│   ├── inbox.mdx
│   ├── scopes.mdx
│   ├── agent-action-log.mdx
│   ├── connect-outbound.mdx
│   └── recipes.mdx
│
├── skills/
│   ├── chartjs.mdx           # Data visualization
│   ├── kanban.mdx            # Drag-drop boards
│   ├── weavy.mdx             # Collaboration features
│   └── design.mdx            # Themes and styling
│
├── mcp/                      # The Gaia MCP connector
│   ├── overview.mdx          # What it is, and coming from the CLI
│   ├── claude-code.mdx       # Setup in Claude Code
│   ├── claude.mdx            # Setup in Claude chat, Cowork, Desktop
│   ├── codex.mdx             # Setup in OpenAI Codex
│   ├── tools.mdx             # Every tool and parameter
│   └── datasets.mdx          # Recurring data pipelines
│
├── compare/                  # vs Retool, Appsmith, Lovable, Replit, Structify
│
└── reference/
    ├── api.mdx               # Generated app APIs
    └── troubleshooting.mdx   # Common issues
```

## Local Development

1. Install the Mintlify CLI:
   ```bash
   npm i -g mintlify
   ```

2. Run the local development server:
   ```bash
   mintlify dev
   ```

3. Open `http://localhost:3000` to preview the docs.

## Deployment

Documentation is automatically deployed when changes are pushed to the main branch.

## Contributing

1. Make changes to the relevant `.mdx` files
2. Preview locally with `mintlify dev`
3. Create a pull request

## Resources

- [Mintlify Documentation](https://mintlify.com/docs)
- [Gainable App](https://build.gainable.dev)
