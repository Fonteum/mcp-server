# Fonteum MCP Server

[![MCP Badge](https://lobehub.com/badge/mcp/fonteum-mcp-server)](https://lobehub.com/mcp/fonteum-mcp-server)

Hosted, read-only Model Context Protocol (MCP) access to Fonteum's
source-linked public-records data graph.

Fonteum covers US healthcare, federal procurement, sanctions and watchlists,
federal enforcement, and global open-data corporate registers. **111 active
source families in the provenance ledger as of 2026-07-12.** Where a source
supports it, results identify the official source, the date Fonteum captured
the record, and relevant coverage or limitation details.

The service returns dated public-record facts. It does not issue a clearance,
risk score, or verdict. Re-confirm a material match with the issuing authority
before acting on it.

## Connect

- **Hosted endpoint:** `https://fonteum.com/api/mcp`
- **Transport:** Streamable HTTP
- **Website:** [fonteum.com](https://fonteum.com)

### Claude Code

```bash
claude mcp add --transport http fonteum https://fonteum.com/api/mcp
```

### Claude Desktop, Cursor, or Windsurf

Add the hosted server to your MCP configuration:

```json
{
  "mcpServers": {
    "fonteum": {
      "type": "http",
      "url": "https://fonteum.com/api/mcp"
    }
  }
}
```

### Local stdio package

The same seven-tool contract is published as
[`@fonteum/mcp` 0.3.0](https://www.npmjs.com/package/@fonteum/mcp):

```bash
claude mcp add fonteum -- npx -y @fonteum/mcp@0.3.0
```

## Tools

All seven tools are read-only.

| Tool | What it does |
| --- | --- |
| `fonteum_resolve_entity` | Resolves an entity by NPI, UEI, or CAGE and returns available healthcare or federal-procurement records with source context. |
| `fonteum_search_records` | Searches US healthcare records by attributes such as vertical, state, county, name, or specialty. |
| `fonteum_check_exclusions_and_sanctions` | Checks an NPI or name against applicable US exclusion, debarment, and sanctions lists. Re-confirm material matches with the issuing authority. |
| `fonteum_get_record_as_of` | Returns a federal contractor record as captured on a supplied date, using a UEI or CAGE identifier. |
| `fonteum_recheck` | Returns source and capture information to support a follow-up check of a Fonteum record. |
| `fonteum_list_sources` | Lists Fonteum's public multi-vertical source catalog and separately reports the dated active-ledger count, with authority, coverage, refresh information, and official source URLs. |
| `fonteum_dataset_info` | Returns published methodology, scope, and source-catalog metadata for the current MCP service. |

## Scope notes

Record availability differs by source family and jurisdiction. The tools expose
the source context that is available for each returned record; absence of a
result is not a determination about a person or organization.

For the same product contract through REST and OpenAPI, use Fonteum's
documentation at [fonteum.com](https://fonteum.com).

## Authorship and medical review

This repository and the `@fonteum/mcp` package are maintained by **Fonteum
LLC**. Medical review: **Dr. Jennifer Montecillo, MD**.

Contact: [hello@fonteum.com](mailto:hello@fonteum.com)

## License

MIT. See [LICENSE](./LICENSE).
