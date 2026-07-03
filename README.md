# QorenextMCP

> Company hierarchy analysis and address verification — via MCP

QorenextMCP is a [Model Context Protocol](https://modelcontextprotocol.io) server that enables Claude and other AI clients to perform company hierarchy analysis and address verification directly in chat.

---

## Get your API key

Sign up at **https://qorenext-app.azurewebsites.net/signup** to get your `QORENEXT_API_KEY`.

---

## Connect

### Claude Code
```bash
claude mcp add --transport http qorenext-mcp \
  "https://mcp.qorenext.com/mcp" \
  --header "X-API-Key: YOUR_API_KEY"
```

### Claude Desktop
Edit `%APPDATA%\Claude\claude_desktop_config.json` (Windows)
or `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac):

```json
{
  "mcpServers": {
    "qorenext-mcp": {
      "url": "https://mcp.qorenext.com/mcp",
      "headers": {
        "X-API-Key": "YOUR_API_KEY",
        "Content-Type": "application/json",
        "Accept": "application/json"
      }
    }
  }
}
```

### Cursor / Windsurf
Add to `.cursor/mcp.json` or `.windsurf/mcp.json`:

```json
{
  "mcpServers": {
    "qorenext-mcp": {
      "url": "https://mcp.qorenext.com/mcp",
      "headers": {
        "X-API-Key": "YOUR_API_KEY"
      }
    }
  }
}
```

---

## Tools

| Tool | Auth | Description |
|---|---|---|
| `health_check` | ❌ Public | Verify server is running, get version info |
| `submit_entities` | ✅ API Key | Submit companies for address verification or hierarchy creation |
| `get_request_status` | ✅ API Key | Poll status and retrieve results of any screening request |

---

### `health_check`
Verify the server is running. No API key required.
```
check if QorenextMCP is running
```

---

### `submit_entities`
Submit companies for Hierarchy creation or Address verification.

**Required per entity:** `entityName`, `country`, `address`, `screeningType` (`"Hierarchy"` or `"Address verification"`)

**Optional:** `crmid`, `website`

```
submit Acme Corp from USA for Hierarchy creation
submit Apple Inc from USA at 1 Apple Park Way, Cupertino for Address verification
submit Samsung from South Korea at Samsung Tower, Seoul for Address verification
```

---

### `get_request_status`
Poll a screening request for status and results.

**Parameter:** `request_id` (int)

**Status values:** `PENDING` · `PROCESSING` · `COMPLETE` · `FAILED`

```
get status of request 1001
check if screening 5042 is complete
```

---

## Example workflow

```
You:    Submit Acme Corp from USA for Hierarchy creation.

Claude: [calls submit_entities]
        ✅ Submitted. Entity ID: 1042

You:    Get status of request 1042

Claude: [calls get_request_status]
        Status: COMPLETE
        Hierarchy: Acme Corp → Acme Holdings (USA) → GlobalCorp (UK)
```

---

## Support

- Issues: https://github.com/QoreNext-IT/QoreNextMCP/issues
- Email: support@qorenext.com
- Docs: https://qorenext.com

---

## License

MIT
