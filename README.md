# RankRoot AI Visibility Check (GitHub Action)

Audit how visible your site is to **ChatGPT, Perplexity, Claude, Google AI Overviews and AI agents** on every deploy, and fail CI if it regresses (e.g. someone ships a robots.txt that blocks AI search bots, removes your JSON-LD, or turns the homepage into a JS-only shell).

```yaml
name: AI visibility
on:
  push:
    branches: [main]
  schedule:
    - cron: "0 6 * * 1"
jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: zachhhhh/rankroot-action@v1
        with:
          url: https://yourproduct.com
          min-score: 70
          # api-key: ${{ secrets.RANKROOT_KEY }}   # optional, Pro/Agency key for higher limits
```

The job summary shows the score, category breakdown and every failing check.

Checks include: AI search & training crawler access (OAI-SearchBot, ChatGPT-User, PerplexityBot, Claude-SearchBot, GPTBot, ClaudeBot…), content readable without JavaScript, title/description/headings, FAQ content, schema.org entity data, llms.txt, OpenAPI/MCP discovery, pricing and docs.

Free: 20 audits per 10 minutes per runner IP. Higher limits and full-site (multi-page) audits with [RankRoot Pro](https://getrankroot.com/#pricing).

Web app: https://getrankroot.com · MCP server: `https://getrankroot.com/api/mcp`

## Publish fixes as a pull request

For RankRoot's AI Visibility Employee plan. When you approve fixes in your RankRoot dashboard (pages that answer the questions buyers ask AI assistants, an FAQ, llms.txt), this action writes them into your repo in the form your site uses and opens one pull request. You review, edit and merge.

```yaml
name: RankRoot fixes
on:
  schedule:
    - cron: "0 7 * * *"   # daily; nothing happens when there's nothing new
  workflow_dispatch:
permissions:
  contents: write
  pull-requests: write
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: zachhhhh/rankroot-action/publish@v2
        with:
          api-key: ${{ secrets.RANKROOT_KEY }}
```

One-time setup: add your license key as the repository secret `RANKROOT_KEY`, and allow Actions to open pull requests (Settings > Actions > General > Workflow permissions > "Allow GitHub Actions to create and approve pull requests").

What gets written:

| Your site | New pages go to |
| --- | --- |
| Next.js App Router | `app/<path>/page.tsx` (or `src/app/...`), server-rendered, with metadata and JSON-LD |
| Next.js Pages Router | `pages/<path>.tsx` |
| Anything else (Vite, Astro, Gatsby, Nuxt, static) | `public/<path>/index.html`, plain HTML that AI crawlers can read even if the rest of your site is a client-rendered app |

It never overwrites a file you already have: existing pages, `robots.txt` and `llms.txt` are left alone and listed in the pull request instead. RankRoot checks your live site weekly and starts measuring each page's effect from the day it goes live.

License: MIT
