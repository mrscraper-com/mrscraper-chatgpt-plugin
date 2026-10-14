# MrScraper for ChatGPT and Codex

This marketplace packages MrScraper's MCP-oriented skills together with the
hosted MrScraper MCP server. It is a test distribution for ChatGPT Work mode,
the ChatGPT desktop app, and Codex.

## Add the marketplace

Use the GitHub `owner/repo` value as the marketplace source:

```text
mrscraper-com/mrscraper-chatgpt-plugin
```

If the client specifically asks for a Git repository URL, use
`https://github.com/mrscraper-com/mrscraper-chatgpt-plugin.git`.

From Codex CLI, add the marketplace and install the plugin:

```bash
codex plugin marketplace add mrscraper-com/mrscraper-chatgpt-plugin
codex plugin add mrscraper@mrscraper-plugins
```

The plugin connects to `https://mcp.mrscraper.com/mcp`. When prompted, finish
the OAuth authorization flow in your browser. Do not paste OAuth tokens or API
keys into chat.

## Try it

```text
Fetch https://www.scrapethissite.com/pages/simple/ and summarize the page.
```

The hosted server exposes three tools: `scraperAPI` retrieves a known page as
HTML, Markdown, a screenshot, or a combination; `webSearch` searches Google;
and `status` reports account usage. The included skills use `scraperAPI` as the
normal first step when a URL is known because it preserves the page response
for flexible agent-led analysis.

## Contents

- `plugins/mrscraper/.codex-plugin/plugin.json`: ChatGPT/Codex plugin manifest
- `plugins/mrscraper/skills/`: MCP-oriented MrScraper skill pack
- `.agents/plugins/marketplace.json`: repository marketplace catalog

## Submission packaging

Package the contents of `plugins/mrscraper`, not the marketplace repository.
The ZIP root must contain `.codex-plugin/plugin.json`, `.mcp.json`, `assets/`,
and `skills/`. Increment the plugin manifest version when listing metadata
changes; this version is independent of the hosted MCP server version.

The listing uses the `Developer Tools` category and
<https://mrscraper.com/support> for customer support. Keep the description and
capability labels aligned with the actual tools.

The privacy-policy URL points to the published policy at
<https://mrscraper.com/privacy-policy>. A new ZIP does not update that page.
Before resubmitting, ensure the published policy covers MCP inputs, scraped
content, backend model processing, recipient categories, retention timelines,
and available user controls.

### Privacy-policy preview package

`mrscraper-0.1.5-privacy-preview.zip` is a separate metadata-check package. It
changes only the embedded version (`0.1.5`) and `privacyPolicyURL` from version
`0.1.4`, pointing to the proposed policy at
<https://mrscraper-privacy-preview-20261005.subscriptions-7de.workers.dev/privacy-policy>.
The production policy and source manifest remain unchanged. The preview is
deployed from landing-v3's isolated `preview/privacy-policy-20261005` branch;
do not merge that branch's preview-only deployment workflow into `develop`.
After publishing the approved policy, package the permanent URL with a newer
version and run the final checks. A successful preview check is not approval.

## Development validation

```bash
codex plugin marketplace add .
codex plugin list
```

This repository is for marketplace testing before the equivalent packaging is
integrated into the official MrScraper repositories.
