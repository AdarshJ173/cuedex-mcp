# Cuedex MCP

Connect your AI agent to [Cuedex](https://cuedex.dev): video memory infrastructure that lets agents search YouTube channels you trust and answer with cited video timestamps.

- Live endpoint: `https://api.cuedex.dev/mcp`
- Auth: Bearer API key (minted at [cuedex.dev/console/keys](https://cuedex.dev/console/keys)) or OAuth for Claude Web
- Scopes: search plus timestamped context excerpts across your collections

## Setup

### 1. Mint a key

1. Open [cuedex.dev/console/keys](https://cuedex.dev/console/keys) and create a key (scope `read` is enough for search).
2. Copy it once. It is shown only at mint time.

### 2. Point your agent at it

Claude Code:

```bash
claude mcp add cuedex --transport http https://api.cuedex.dev/mcp \
  --header "Authorization: Bearer YOUR_KEY"
```

Cursor, OpenCode, Claude Desktop (`mcpServers` JSON):

```json
{
  "mcpServers": {
    "cuedex": {
      "url": "https://api.cuedex.dev/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

Claude Web: Settings, Connectors, Add custom connector, enter `https://api.cuedex.dev/mcp`, approve read access when asked. No JSON pasting.

### 3. Ask

> Search my library for how startups should price their product, with timestamps.

The agent queries your collections and returns timestamped excerpts with clickable video links. Attach channels first from [cuedex.dev/console/collections](https://cuedex.dev/console/collections), including free pre-indexed packs.

## Registry

`server.json` in this repo follows the MCP Registry schema so directories and LLMs resolve this connector to the live endpoint above.

## Links

- Product: [cuedex.dev](https://cuedex.dev)
- Docs: [cuedex.dev/docs](https://cuedex.dev/docs)
- Console: [cuedex.dev/console](https://cuedex.dev/console)
