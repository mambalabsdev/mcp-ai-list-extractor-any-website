# AI List Extractor for Any Website MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-ai-list-extractor-any-website/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-ai-list-extractor-any-website) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-ai-list-extractor-any-website%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-ai-list-extractor-any-website&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-ai-list-extractor-any-website)](https://www.npmjs.com/package/@mambalabsdev/mcp-ai-list-extractor-any-website) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-ai-list-extractor-any-website)](https://www.npmjs.com/package/@mambalabsdev/mcp-ai-list-extractor-any-website) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-ai-list-extractor-any-website)](https://github.com/mambalabsdev/mcp-ai-list-extractor-any-website/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-ai-list-extractor-any-website)

An MCP server for the Mamba Labs AI List Extractor for Any Website actor on Apify. Turn any public list page into rows on your own LLM key.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give web addresses, a plain words description of one item, and your own OpenAI, Anthropic, Gemini, or Groq key. The actor reads each page over plain HTTP, sends its text to your model, and returns one row per item, with a check that flags values not found on the page. You pay your provider directly and pay this actor per page. It does not run JavaScript and does not get past a refusal.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-ai-list-extractor-any-website": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-ai-list-extractor-any-website"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `extract_list_from_website` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Pull every job title, company, and link from this careers page on my OpenAI key."
- "Extract all products and prices from this category page, following 5 pages."
- "Check whether these 3 pages can be read before I use my key."

## Tool

`extract_list_from_website`. One tool. Example arguments:

```json
{
  "start_urls": [
    "https://quotes.toscrape.com/"
  ],
  "what_to_extract": "every quote: text, author, tags",
  "provider": "openai",
  "max_pages_per_url": 1
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `start_urls` | array | no | Public list pages that answer without a login, for example a careers page or a category page. A page that needs JavaScript or refuses plain requests returns a free row. |
| `what_to_extract` | string | no | Plain words, for example: every job: title, company, city, link. Needed when a key is given. |
| `fields` | array | no | Exact keys each item must carry, for the same columns on every page. Empty lets the model choose. In Clay, wrap a column in a one item list. |
| `provider` | string | no | Whose API reads the page, on your key: openai (default), anthropic, gemini, or groq. |
| `api_key` | string | no | Your own key. It goes only to the provider you picked and is never stored, logged, or sent to the model. With no key the actor reads up to 3 addresses for free and sends nothing to a model. |
| `model` | string | no | Your provider's model name. Defaults: openai gpt-4o-mini, anthropic claude-haiku-4-5-20251001, gemini gemini-2.5-flash, groq llama-3.3-70b-versatile. |
| `max_pages_per_url` | number | no | How many pages to read from each address by following next page links. Default 3, at most 100. Each page your model answers is charged. |
| `max_items` | number | no | The run stops after this many items. Default 1000. |
| `max_chars_per_page` | number | no | Page text past this length is not sent, which limits your provider bill. Default 40000. The row says when text was cut. |
| `obey_robots` | boolean | no | yes (default) or no. A disallowed page returns a free row. |
| `max_charge_usd` | string | no | Stops before a page would pass this figure, before your model is called. This limits this actor's charges, not your provider's bill. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `page-extracted`, one page your model answered, whatever the number of items on it | $0.01 | $0.0095 | $0.009 | $0.0085 |
| `apify-actor-start`, per run start | $0.012 | $0.012 | $0.012 | $0.012 |

Items behind a charged page, refused pages, failed provider calls, runs with no key, and refused input are free. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/ai-list-extractor-any-website](https://apify.com/mambalabs/ai-list-extractor-any-website)

## License

MIT. See [LICENSE](LICENSE).
