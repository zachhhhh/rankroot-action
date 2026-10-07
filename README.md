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

Free: 20 audits per 10 minutes per runner IP. Higher limits and full-site (multi-page) audits with [RankRoot Pro](https://rankroot-ai.netlify.app/#pricing).

Web app: https://rankroot-ai.netlify.app · MCP server: `https://rankroot-ai.netlify.app/api/mcp`

License: MIT
