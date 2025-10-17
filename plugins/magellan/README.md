# Magellan Plugin

Toolkit for running exploratory browser voyages with Claude Code.

## Components

- `/voyage` slash command (project scope) that orchestrates destination-based test routes using `voyages/chart.yaml`, `voyages/maps.yaml`, and Markdown route files such as `voyages/ruth.md`.
- `/a` slash command for quick delegation to any subagent (`/a elkano <instruction>`).
- `elkano` subagent configured to execute Playwright MCP actions precisely as requested and to return concise status updates or locator snippets.

## Expected project setup

The command assumes your repository contains:

```
voyages/
├── chart.yaml      # environment variables (e.g., base URLs, credentials)
├── maps.yaml       # stable element locators (role/name, text, or selector)
└── *.md            # voyage route files with "Destination:" headings and plan steps
```

If these files are missing, `/voyage` will prompt you. You can commit templates alongside this plugin or generate them per project.

## Installation

Add the marketplace and install:
```
/plugin marketplace add UKGEPIC/claude-code-resources#magellan
/plugin install magellan@ukg
```

Restart Claude Code, then run `/voyage <route-name>` to test.

## Notes

- The Playwright MCP server is automatically configured when you install this plugin.
- Elkano never emits raw MCP payloads—only terse status messages or YAML snippets when discovering new map entries.
