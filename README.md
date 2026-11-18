# MorningsideMunch MCP Server

An [MCP](https://modelcontextprotocol.io) server that gives agents live Columbia dining data from MorningsideMunch: which halls are open, their hours and full menus with dietary labels and allergens.

It's hosted, read-only and needs no API key:

```
https://morningsidemunch-mcp.vercel.app/mcp
```

## Tools

| Tool | What it does |
| --- | --- |
| `list_halls` | Every location, with open-now status, today's hours, meal periods and how many menu items are posted. Filter by `open_now`. |
| `get_menu` | One hall's menu by meal and station. Takes a hall id or a loose name ("JJs", "ferris"), plus optional `meal`, `date`, `dietary` and `exclude_allergens`. |
| `search_menu` | Searches dishes across all halls by text and/or dietary filters, e.g. halal chicken at dinner, or vegan and dairy-free food at halls that are open now. |
| `get_hours` | Hours, open-now and the next open or close time for one hall or all of them. |
| `get_news_events` | Columbia Dining news and events. |

Dietary filters: `halal`, `kosher`, `vegan`, `vegetarian` (also matches vegan items), `gluten_free`. Dates are `YYYY-MM-DD`, and all times are America/New_York.

## Setup

### Claude Code

```bash
claude mcp add --transport http morningsidemunch https://morningsidemunch-mcp.vercel.app/mcp
```

### Claude (web and desktop)

Settings → Connectors → Add custom connector, and paste the URL above.

### Cursor, VS Code and other clients

```json
{
  "mcpServers": {
    "morningsidemunch": {
      "url": "https://morningsidemunch-mcp.vercel.app/mcp"
    }
  }
}
```

For clients that only support local (stdio) servers, bridge it with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "morningsidemunch": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://morningsidemunch-mcp.vercel.app/mcp"]
    }
  }
}
```

## Data

Status, hours and menus come from Columbia Dining's own published schedules, so breaks, holidays and event closures show up automatically. Locations with no published hours report `openNow: null`. It's the same data the MorningsideMunch app shows.

## License

MIT. See [LICENSE](LICENSE).
