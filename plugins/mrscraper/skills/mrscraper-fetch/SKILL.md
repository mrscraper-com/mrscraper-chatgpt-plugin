---
name: mrscraper-fetch
description: |
  Retrieve a known public URL with MrScraper's scraperAPI tool as HTML, Markdown, a screenshot, or a combination. Use for reading, summarizing, archiving, or analyzing a page and before local extraction; supports automatic browser rendering, waiting for delayed content, and loading a page from a chosen country. Use mrscraper-serp when no URL is known.
---

# Retrieve Page Content with MrScraper MCP

Use the scraperAPI tool supplied by the MrScraper MCP server whenever the user
already has a public URL. Keep using the retrieved content for summarization,
comparison, transformation, and structured output. Use
[mrscraper](../mrscraper/SKILL.md) for connection and authentication checks,
account status, or broader routing, and
[mrscraper-serp](../mrscraper-serp/SKILL.md) when discovery must happen first.

## Page Retrieval Context

scraperAPI loads the page once through MrScraper. Browser rendering executes
page JavaScript, a selector wait lets delayed content appear, and geo_code
loads the page from a chosen country.

Use scraperAPI only for publicly accessible pages the user is allowed to read,
and follow the site's terms. scraperAPI does not sign in to websites. When a
page requires signing in, or the site refuses the request, tell the user and
stop instead of trying to work around it.

Start with the URL alone and add only the controls the page needs. Plan-token
usage is based on runtime and bandwidth: one token per 30 seconds and one token
per 0.25 MB, rounded up per component. See the
[Token Plan](https://docs.mrscraper.com/docs/getting-started/api-token) for the
maintained calculation.

## Step 1 — Define the Outcome

Confirm the target URL and what the user wants:

- Read, summarize, cite, or inspect the page.
- Check whether specific text appears.
- Archive the page or capture a screenshot.
- Produce fields, JSON, tables, or other structured output with local logic.
- Load JavaScript-rendered or country-specific content.

When many pages share a layout, retrieve representative pages, define one local
extractor, retrieve the remaining pages in parallel when safe and proportional,
and apply the same extractor to every saved response.

## Step 2 — Retrieve the Page

The client may namespace the tool name; select scraperAPI from the MrScraper
MCP provider. Start with one call containing only the required URL, which
requests HTML:

    {
      "url": "https://www.scrapethissite.com/pages/simple/"
    }

Request Markdown instead of HTML for readable text:

    {
      "url": "https://www.scrapethissite.com/pages/simple/",
      "html": false,
      "markdown": true
    }

The result is available in MCP structuredContent and as formatted JSON text.
The requested representations are in data:

    {
      "status_code": 200,
      "data": {
        "html": "<!DOCTYPE html>...",
        "markdown": null,
        "screenshot": null,
        "token_usage": 3,
        "runtime": 18.02,
        "error": null
      },
      "headers": {
        "x-status-code": "200"
      }
    }

Prefer structuredContent. Check isError, error, status_code, and
headers["x-status-code"] (the target page's status) before using data.

A screenshot is a base64 JPEG that can be large. Request it only when the user
needs an image, and save it to a file rather than reading it into the
conversation:

    {
      "url": "https://www.scrapethissite.com/pages/simple/",
      "html": false,
      "screenshot": true
    }

## Step 3 — Choose Page-Loading Options

Omit browser_rendering to let the service choose: it falls back to a browser
automatically when needed. true forces a browser and false disables it.
screenshot=true always uses a browser.

Wait for delayed content with a CSS selector:

    {
      "url": "https://www.scrapethissite.com/pages/ajax-javascript/#2015",
      "browser_rendering": true,
      "wait_for_selector": ".film"
    }

Load the page from another country when its content differs by country:

    {
      "url": "https://www.scrapethissite.com/pages/simple/",
      "geo_code": "ID"
    }

### Parameters

| MCP input | Default | Use |
| --- | --- | --- |
| url | required | Absolute HTTP or HTTPS page URL. |
| html | true | Request HTML content. |
| markdown | false | Request Markdown content. |
| screenshot | false | Capture a base64 JPEG; forces browser rendering. |
| browser_rendering | automatic | true forces and false disables browser rendering. |
| wait_for_selector | omitted | CSS selector to wait for before returning. |
| geo_code | US | Country to load the page from, such as US, ID, or SG. |
| timeout | 120 | Page timeout from 1 to 300 seconds. |

Authentication is handled by the MCP client's OAuth 2.1 connection and is
never a tool input.

## Step 4 — Inspect and Retry Deliberately

Start with the URL alone unless the task already establishes a requirement. If
the content is incomplete or missing dynamic content:

1. Inspect status_code, data.error, headers["x-status-code"], and the content.
2. Change one option at a time: force browser_rendering, or add
   wait_for_selector for content that loads late.
3. Set geo_code only when evidence shows the page differs by country.
4. Stop after a usable response unless the user requests a comparison.

When the site refuses the request or asks the visitor to sign in, report that
to the user and stop. Do not repeat an identical request. Treat
wait_for_selector as a CSS selector, not a duration. Browser rendering loads a
page; it does not click controls, submit forms, or provide an authenticated
browser session.

## Step 5 — Deliver the Result

Answer the user's request from data. Keep the full envelope when headers or
diagnostics matter. When the user requests an archive, save the successful
response or the requested field with the environment's local file-writing
capability; scraperAPI itself has no output-path input.

Keep retrieved content as the source of truth for later steps. Build
summaries, tables, JSON transformations, and extraction scripts from it, and
retrieve the page again when a follow-up needs details that an earlier
transformation did not preserve.
