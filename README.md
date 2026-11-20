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

The server exposes seven tools:

| Tool | Description |
| --- | --- |
| `status` | Return the raw account status API response without summarizing, redacting body fields, or adding calculated fields. |
| `scrapeHtml` | Retrieve the raw HTML of a page. Browser rendering is optional. |
| `scrapeMarkdown` | Retrieve a page as Markdown. Including HTML also enables browser rendering and returns both outputs as JSON. |
| `getScreenshot` | Capture a base64 screenshot of the full page or top viewport. Browser rendering is always enabled. |
| `scrapeBrand` | Extract a website's colors, typography, logos, and social links as JSON. |
| `scrapeJson` | Extract structured JSON from a page using a natural-language prompt. Append Json Schema: and a JSON schema to shape the output. |
| `webSearch` | Search the web and return ranked results with titles, URLs, descriptions, and sources. |

The included skills use `scrapeMarkdown`, or `scrapeHtml` when the page
structure matters, as the normal first step when a URL is known because they
preserve the page response for flexible agent-led analysis. The skills read
only public pages the user is allowed to read, never sign in to websites, and
stop and tell the user when a site refuses the request or requires signing in.

## Contents

- `plugins/mrscraper/.codex-plugin/plugin.json`: ChatGPT/Codex plugin manifest
- `plugins/mrscraper/skills/`: MCP-oriented MrScraper skill pack
- `.agents/plugins/marketplace.json`: repository marketplace catalog

## Submission packaging

Package the contents of `plugins/mrscraper`, not the marketplace repository.
The ZIP root must contain `.codex-plugin/plugin.json`, `.mcp.json`, `assets/`,
and `skills/`. Increment the plugin manifest version when listing metadata
changes; this version is independent of the hosted MCP server version.

The listing uses the `Productivity` category and
<https://mrscraper.com/support> for customer support. Keep the description and
capability labels aligned with the actual tools.

The review test cases (five positive, three negative) and the release notes
are in `extensions.com.openai` of the plugin manifest. The dashboard imports
them from the ZIP and shows them read-only, so change them in the manifest and
upload a new version. The demo video URL and reviewer credentials stay in the
dashboard.

## Changes in 0.4.0

- The plugin follows MrScraper MCP server 0.5.0 and its seven standard tools.
  `scrapeHtml`, `scrapeMarkdown`, and `getScreenshot` replace `scraperAPI`;
  `scrapeBrand` and `scrapeJson` are new; and `webSearch` takes `query`,
  `country`, and `numResults`.
- The skills are MrScraper's canonical MCP skills with this plugin's review
  rules. The page skill covers only page-loading and output inputs and leaves
  the others at their defaults. It reads only public pages the user is allowed
  to read, never signs in, and stops and tells the user when a site refuses the
  request or requires signing in.
- The review test cases use the new tools and include JSON extraction and
  brand cases. OpenAI accepts exactly five positive cases, so `status` is no
  longer one of them. The listing adds the two new capabilities.
- MrScraper MCP 0.5.0 asks for `scrape:read` (scraping and search) and
  `account:read` (`status`). Connections that already hold `scrape:read` keep
  working; `scrape:write` is no longer needed.

## Changes in 0.3.2

- Added the review test cases and release notes to the manifest. The
  dashboard now requires the test cases to come from the ZIP.

## Changes in 0.3.1

- The plugin connects to `https://mcp.mrscraper.com/mcp` again. OpenAI's
  plugin directory doesn't allow an existing plugin to change its MCP URL.
  MrScraper MCP 0.4.1 gives ChatGPT and Codex sign-ins on `/mcp` the same
  page-loading-only `scraperAPI` as `/mcp/chatgpt`.

## Changes in 0.3.0

- The plugin connects to `https://mcp.mrscraper.com/mcp/chatgpt` instead of
  `/mcp`. That endpoint uses the same sign-in, and its `scraperAPI` has only
  page-loading options.
- The listing describes web search and reading public pages for research. It
  no longer mentions Google or developer data pipelines, and it moved from
  `Developer Tools` to `Productivity`.
- The skills follow MrScraper MCP 0.3.0: `webSearch` returns a ranked list and
  takes `num_results`. The page skill covers only the endpoint's options and
  tells the agent to stop when a site refuses the request or requires signing
  in.

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
