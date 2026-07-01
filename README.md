# QornextMCP

> Company verification, hierarchy analysis, trade screening and sanctions compliance — via MCP

QornextMCP is a [Model Context Protocol](https://modelcontextprotocol.io) server that lets Claude and other AI clients perform company verification, organizational hierarchy analysis, and trade/sanctions screening directly in chat.

---

## Get your API key

Sign up at **https://qorenext-app.azurewebsites.net/signup** to get your `QORNEXT_API_KEY`.

---

## Connect

### Claude Code
```bash
claude mcp add --transport http qornext-mcp \
  "https://mcp.qorenext.com/mcp" \
  --header "X-API-Key: YOUR_API_KEY"
```

### Claude Desktop
Edit `%APPDATA%\Claude\claude_desktop_config.json` (Windows)
or `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac):

```json
{
  "mcpServers": {
    "qornext-mcp": {
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
    "qornext-mcp": {
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
| `submit_entities` | ✅ API Key | Submit companies for verification or hierarchy analysis |
| `submit_company_for_screening` | ✅ API Key | Submit company for trade screening and sanctions check |
| `get_request_status` | ✅ API Key | Poll status and retrieve results of any screening request |

---

### `health_check`
Verify the server is running. No API key required.
```
check if QornextMCP is running
```

---

### `submit_entities`
Submit companies for Hierarchy or Verification screening.

**Required per entity:** `entityName`, `country`, `screeningType` (`"Hierarchy"` or `"Verification"`)

**Optional:** `crmid`, `address1`, `city`, `stateProvince`, `postalCode`, `website`

```
submit Acme Corp from USA for Hierarchy screening
submit Apple Inc (USA) and Samsung (South Korea) for Verification
```

---

### `submit_company_for_screening`
Submit a company for trade and sanctions compliance screening.

**Required:** `assembly_line_id`, `subscription_id`, `company_name`, `reference_number`

**Optional:** `english_name`, `address`, `website`, `departments`, `labs`,
`professors`, `products`, `end_use`, `parent_entity`, `parent_country`,
`priority_level` (`low`/`normal`/`high`/`urgent`), `is_university`

```
screen Huawei Technologies, assembly line AL-001,
subscription SUB-123, reference REF-2026-001, priority high
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
You:    Submit Acme Corp from USA for Hierarchy screening

Claude: [calls submit_entities]
        ✅ Submitted. Entity ID: 1042

You:    Get status of request 1042

Claude: [calls get_request_status]
        Status: COMPLETE
        Hierarchy: Acme Corp → Acme Holdings (USA) → GlobalCorp (UK)
        Sanctions: Not flagged (OFAC, EU, UN checked) ✅
```

---

## Support

- Issues: https://github.com/QoreNext-IT/QoreNextMCP/issues
- Email: support@qorenext.com
- Docs: https://qorenext.com/docs

---

## License

MIT
