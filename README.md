# MrScraper for ChatGPT and Codex

This marketplace packages MrScraper's MCP-oriented skills together with the
hosted MrScraper MCP server. It is a test distribution for ChatGPT Work mode,
the ChatGPT desktop app, and Codex.

## Add the marketplace

Use this GitHub repository as the marketplace source:

```text
https://github.com/pray-mrscraper/mrscraper-chatgpt-plugin
```

From Codex CLI, add the marketplace and install the plugin:

```bash
codex plugin marketplace add pray-mrscraper/mrscraper-chatgpt-plugin
codex plugin add mrscraper@mrscraper-plugins
```

The plugin connects to `https://mcp.mrscraper.com/mcp`. When prompted, finish
the OAuth authorization flow in your browser. Do not paste OAuth tokens or API
keys into chat.

## Try it

```text
Fetch https://www.scrapethissite.com/pages/simple/ and summarize the page.
```

The included skills encourage `fetch` as the normal first step when a URL is
known because it preserves the page response for flexible agent-led analysis.
Use `scrape` when you specifically need backend structured extraction,
pagination, schema-shaped output, or a reusable saved scraper.

## Contents

- `plugins/mrscraper/.codex-plugin/plugin.json`: ChatGPT/Codex plugin manifest
- `plugins/mrscraper/skills/`: MCP-oriented MrScraper skill pack
- `.agents/plugins/marketplace.json`: repository marketplace catalog

## Development validation

```bash
codex plugin marketplace add .
codex plugin list
```

This repository is for marketplace testing before the equivalent packaging is
integrated into the official MrScraper repositories.
