# Fonteum MCP Server

[![MCP Badge](https://lobehub.com/badge/mcp/fonteum-mcp-server)](https://lobehub.com/mcp/fonteum-mcp-server)

Hosted, read-only Model Context Protocol (MCP) access to Fonteum's public-records and enforcement-integrity platform.

Fonteum organizes public records from the United States and supported global open-data registers. The service covers healthcare, federal procurement, sanctions and watchlists, federal enforcement, intellectual property, securities filings, and company registers. It returns the records available for a request together with the source and timing information available for that result.

Fonteum reports public-record facts. It does not issue clearances, risk scores, labels, or verdicts. Re-check a material match with the issuing authority before acting on it.

## Connect

- **Hosted endpoint:** `https://fonteum.com/api/mcp`
- **Transport:** Streamable HTTP
- **Product documentation:** [fonteum.com](https://fonteum.com)
- **npm package:** [`@fonteum/mcp`](https://www.npmjs.com/package/@fonteum/mcp)

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

The hosted endpoint permits anonymous, rate-limited access. If Fonteum gives you a hosted transport key, send it as `x-fonteum-mcp-key`. Do not put credentials in a public configuration file.

## Tools

These are the read-only tools currently exposed by the hosted server.

| Tool | Ask it when you need | What the answer contains |
| --- | --- | --- |
| `fonteum_resolve_entity` | Public records for one NPI, UEI, or CAGE identifier. | Available provider or federal-contractor records tied to that identifier, plus source context for returned facts. |
| `fonteum_search_records` | Healthcare provider records for a provider type and state. | Matching records from the available healthcare search data. It is not a general search across every Fonteum subject area. |
| `fonteum_check_exclusions_and_sanctions` | Whether an NPI or name appears in the supported exclusion, debarment, or sanctions records. | Any matching list entries, the issuing authority, and available source timing. A match needs issuer confirmation. |
| `fonteum_get_record_as_of` | Available federal-contractor records for a specified past date. | Records available for the requested UEI or CAGE and date. Historical coverage depends on the underlying source. |
| `fonteum_recheck` | A way to independently inspect Fonteum's stored verification record. | A current re-check link, or details for one supplied snapshot. This supports inspection of the stored artifact; it does not prove every returned fact. |
| `fonteum_list_sources` | The public sources represented in Fonteum's catalog. | The current catalog entries, their official publishers, source links, update notes, and reuse information when available. |
| `fonteum_dataset_info` | How Fonteum handles sources and what the service can cover. | Current method, scope notes, how to read source details, and the generated source catalog. |

### Inputs in plain language

- **NPI** — the U.S. healthcare-provider identifier.
- **UEI** — the U.S. federal-contractor identifier.
- **CAGE** — a government-assigned code used to identify a contractor or supplier.
- **`as_of`** — the date you want the contractor lookup to use, written as `YYYY-MM-DD`.
- **`snapshot_id`** — an optional identifier for a specific stored Fonteum snapshot to inspect.

## Reading a response

Responses may include these top-level sections. Fields can be absent or `null` when a source does not provide them.

| Section | Plain-language meaning |
| --- | --- |
| `data` | The answer you requested: records, matches, links, or catalog entries. |
| `provenance` | Where an answer came from, when Fonteum handled it, and any available reuse or coverage context. |
| `freshness` | Any available indication of how recently the relevant source or assembled answer was checked. |
| `attestation` | A stored verification record you can inspect independently. It is not a guarantee that every fact is true or complete. |

When `provenance` is present, its fields mean:

| Field | Plain-language meaning |
| --- | --- |
| `_source` | The publisher or Fonteum record set behind this answer. |
| `_source_url` | A link to the original publisher or Fonteum source page. |
| `_dataset_id` | Fonteum's identifier for the source collection used. |
| `_snapshot` | The stored copy or source date tied to this answer, when one is available. |
| `_methodology` | The published method version used for the result, when supplied. |
| `_last_checked` | When Fonteum last checked or assembled this answer. |
| `_confidence` | A system-supplied technical value, when present; it is not a score or recommendation about a person or organization. |
| `_data_availability` | Notes about what source context was present, unavailable, or combined. |
| `_pipeline_version` | The processing release that produced the response. |
| `_doi` | A publication identifier, if the source supplies one. |
| `_license` | Reuse terms supplied for the source, if known. |
| `_coverage_period_start` | The beginning of the time period the source says it covers, if known. |
| `_coverage_period_end` | The end of the time period the source says it covers, if known. |
| `_slsa_provenance_url` | A link to build information when one is emitted. |

An empty answer or missing field means Fonteum did not return that item for this request. It does not establish that the item does not exist elsewhere.

## Scope notes

Record availability, timing, and historical coverage vary by source and jurisdiction. The service only returns source context that is available for the particular result. Use `fonteum_list_sources` or `fonteum_dataset_info` when you need the current catalog rather than relying on a static inventory in documentation.

## Maintainer and medical review

This repository and the `@fonteum/mcp` package are maintained by **Fonteum LLC**. Medical review: **Dr. Jennifer Montecillo, MD**.

Contact: [hello@fonteum.com](mailto:hello@fonteum.com)

## License

MIT. See [LICENSE](./LICENSE).
