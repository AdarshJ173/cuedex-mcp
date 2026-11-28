# Cuedex MCP

Connect your AI agent to [Cuedex](https://cuedex.dev): video memory infrastructure that lets agents search YouTube channels you trust and answer with cited video timestamps.

- Live endpoint: `https://api.cuedex.dev/mcp`
- Auth: Bearer API key (minted at [cuedex.dev/console/keys](https://cuedex.dev/console/keys)) or OAuth for Claude Web
- Scopes: search plus timestamped context excerpts across your collections

[![Install in Cursor](https://cursor.com/install-mcp/button.svg)](https://cursor.com/install-mcp?name=cuedex&config=eyJ1cmwiOiJodHRwczovL2FwaS5jdWVkZXguZGV2L21jcCJ9)

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

Cursor (1-click badge above) or manual `mcpServers` JSON, same shape works for OpenCode (`opencode.json`) and Claude Desktop (`claude_desktop_config.json`):

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

OpenCode (`opencode.json`):

```json
{
  "mcp": {
    "cuedex": {
      "type": "remote",
      "url": "https://api.cuedex.dev/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

Claude Web: Settings, Connectors, Add custom connector, enter `https://api.cuedex.dev/mcp`, approve read access when asked. No JSON pasting.

Perplexity: Settings, Connectors, add a custom MCP connector with server URL `https://api.cuedex.dev/mcp` and your Bearer key. Use it from any chat by selecting the Cuedex connector.

Gemini CLI: install this repo as an extension, then set your key once:

```bash
gemini extensions install https://github.com/AdarshJ173/cuedex-mcp
export CUEDX_API_KEY="YOUR_KEY"
```

### 3. Ask

> Search my library for how startups should price their product, with timestamps.

The agent queries your collections and returns timestamped excerpts with clickable video links. Attach channels first from [cuedex.dev/console/collections](https://cuedex.dev/console/collections), including free pre-indexed packs.

## Tools

| Tool | Title | What it does |
|---|---|---|
| `search_video_memory` | Search Video Memory | Hybrid search over your collections. Returns timestamped, cited passages. |
| `get_transcript_window` | Get Transcript Window | Transcript window around a timestamp as cited excerpts. |
| `list_collections` | List Collections | Every collection with attached channel IDs and per-channel video counts. |
| `attach_youtube_channel` | Attach YouTube Channel | Index a channel, playlist, or video into a collection. |
| `check_ingest_status` | Check Ingest Status | Ingest job progress and per-video failure reasons. |

Search and read tools are read-only by default. Only `attach_youtube_channel` writes, and only into your own collections.

## Security and fair use

- Every transcript excerpt is wrapped in an `<untrusted_video_excerpt>` firewall so agents treat creator content as data, never instructions.
- Creators can opt out any video or channel from indexing. DMCA or takedown requests: `support@cuedex.dev`.
- Keys are scoped (`read`, `ingest`, `admin`) and shown once at mint time. Revoke anytime from the console.

## Registry

`server.json` in this repo follows the MCP Registry schema so directories and LLMs resolve this connector to the live endpoint above. Companion manifests (`.mcp.json`, `.cursor-plugin/plugin.json`, `gemini-extension.json`, `glama.json`) let Cursor, Gemini CLI, and Glama auto-detect it.

## Links

- Product: [cuedex.dev](https://cuedex.dev)
- Docs: [cuedex.dev/docs](https://cuedex.dev/docs)
- Console: [cuedex.dev/console](https://cuedex.dev/console)
