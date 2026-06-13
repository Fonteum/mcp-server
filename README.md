# Fonteum MCP Server

[![MCP Badge](https://lobehub.com/badge/mcp/fonteum-mcp-server)](https://lobehub.com/mcp/fonteum-mcp-server)

**Hosted Model Context Protocol server for the source-provenanced US federal
healthcare provider data graph.**

Fonteum joins the federal healthcare datasets — NPPES, OIG LEIE, CMS PECOS, CMS
Care Compare, CMS Open Payments, HRSA, and more (23 source families) — into one
provider graph keyed on the National Provider Identifier (NPI). This MCP server
lets an LLM or agent search the graph, resolve a provider, check exclusion
status, read dataset methodology, and list the underlying sources, with **every
returned field tied back to its upstream source, snapshot date, and license**
through a fourteen-field provenance contract. No trust badges, no opaque
scores — radical source transparency.

- **Hosted MCP endpoint:** `https://mcp.fonteum.com/api/mcp`
  (also served from the apex at `https://fonteum.com/api/mcp` — same deployment)
- **Discovery document:** `https://fonteum.com/.well-known/mcp.json`
- **Transport:** Streamable HTTP (JSON-RPC 2.0, stateless)
- **Auth:** none required. Anonymous access is rate-limited per IP (30 requests /
  minute). An optional `x-fonteum-mcp-key` header lifts the limit.
- **Scope:** healthcare-only. Read-only — no mutation tools.
- **REST + FHIR + OpenAPI:** the same graph is exposed as a FHIR R4 API
  (`/api/fhir`), a JSON REST API (`/api/v1`), and an OpenAPI 3.1 spec at
  `https://fonteum.com/openapi.json`.

---

## Quickstart

### Claude Code

```bash
claude mcp add --transport http fonteum https://mcp.fonteum.com/api/mcp
```

### Claude Desktop / Cursor / Windsurf (remote MCP)

Add the streamable-HTTP server to your client's MCP config:

```json
{
  "mcpServers": {
    "fonteum": {
      "type": "http",
      "url": "https://mcp.fonteum.com/api/mcp"
    }
  }
}
```

Restart the client, then ask: `List the federal healthcare data sources Fonteum
reconciles, then look up NPI 1003000118.`

### Local stdio alternative

A self-contained local server that wraps the same public graph over stdio ships
as the [`@fonteum/mcp`](https://www.npmjs.com/package/@fonteum/mcp) npm package:

```bash
claude mcp add fonteum -- npx -y @fonteum/mcp
```

The hosted server (this repo) and the npm package expose the **same five tools**,
read the same federal graph, and return the same provenance contract — enforced
by a CI parity gate so the two surfaces cannot drift. The hosted server is the
zero-install path; the npm package is the offline/self-hosted path.

---

## Tools

Every tool result is a JSON envelope `{ "data": …, "provenance": { …14 keys… } }`.
All five tools are **read-only**.

### `fonteum_search_provider`

Search healthcare providers by vertical + state (with an optional county filter),
name, or specialty/taxonomy. Returns up to 100 records (default 25).

- **Input:** `{ "vertical": "dermatologists", "state": "TX", "limit": 5 }`

```jsonc
// fonteum_search_provider { "vertical": "dermatologists", "state": "TX", "limit": 5 }  →
{
  "data": { "vertical": "dermatologists", "state": "TX", "total_in_state": 42, "returned": 5, "hits": [ { "npi": "…", "city": "…", "taxonomy_primary": "…" }, … ] },
  "provenance": { "_source": "CMS NPPES NPI Registry", … }
}
```

### `fonteum_get_provider`

Resolve a single healthcare provider by NPI (10-digit, Luhn-checked) across all
federal sources. Returns the joined record — specialty, taxonomy, location — with
per-field provenance.

- **Input:** `{ "npi": "1003000118" }`

```jsonc
// fonteum_get_provider { "npi": "1003000118" }  →
{
  "data": { "npi": "1003000118", "specialty_display": "Dermatologists", "state": "CA", "city": "…", "snapshot_date": "2026-06-12" },
  "provenance": { "_source": "CMS NPPES NPI Registry", "_source_url": "https://npiregistry.cms.hhs.gov/", "_confidence": 1.0, … }
}
```

### `fonteum_check_exclusion`

Unified "excluded anywhere" check by NPI across the federal OIG List of Excluded
Individuals/Entities (LEIE) and state Medicaid exclusion lists. Returns the
exclusion flag and any matched exclusion records with provenance.

- **Input:** `{ "npi": "1003000118" }`

```jsonc
// fonteum_check_exclusion { "npi": "1003000118" }  →
{
  "data": { "npi": "1003000118", "is_excluded": false, "matches": [] },
  "provenance": { "_source": "OIG LEIE + state Medicaid exclusion lists", "_source_url": "https://oig.hhs.gov/exclusions/exclusions_list.asp", … }
}
```

### `fonteum_dataset_info`

Return the published methodology and metadata for a federal source family
(`nppes`, `oig-leie`, `cms-pecos`, `cms-open-payments`, `cms-care-compare`, …),
including the methodology version, canonical URL, and provenance-contract spec.

- **Input:** `{ "dataset": "nppes" }`

```jsonc
// fonteum_dataset_info { "dataset": "nppes" }  →
{
  "data": { "dataset": "nppes", "methodology_version": "v2026.05.0", "methodology_url": "https://fonteum.com/methodology", "refresh_cadence": "weekly" },
  "provenance": { … }
}
```

### `fonteum_list_sources`

List the federal source families Fonteum reconciles every healthcare-provider
field against (NPPES, OIG LEIE, CMS PECOS, CMS Care Compare, CMS Open Payments,
HRSA HPSA, and more), each with its authority, tier, refresh cadence, and the
official source URL.

- **Input:** none.

```jsonc
// fonteum_list_sources  →
{
  "data": { "sources": [ { "slug": "nppes", "authority": "CMS", "tier": 1, "refresh_cadence": "weekly", "official_url": "https://npiregistry.cms.hhs.gov/" }, … ], "total": 23 },
  "provenance": { "_source": "Fonteum source registry", "_methodology": "v2026.05.0", … }
}
```

---

## The fourteen-field provenance contract

Every tool response carries all fourteen keys, so any fact an agent reads can be
traced to its source, snapshot, methodology, license, coverage window, and signed
build attestation:

```
_source            _source_url         _dataset_id            _snapshot
_methodology       _last_checked       _confidence            _data_availability
_pipeline_version  _doi                _license               _coverage_period_start
_coverage_period_end                   _slsa_provenance_url
```

Field naming matches the Fonteum REST audit-pack endpoint, so tooling that
already consumes the REST API reads MCP responses without translation. The
contract is additive-only — keys are never stripped.

## Healthcare scope

This server is healthcare-only by doctrine. The verticals it resolves are:
chiropractors, dermatologists, plastic-surgeons, med-spas, weight-loss clinics,
rehab centers, hair-transplant clinics, fertility clinics, TRT clinics, and
ketamine clinics. Federal source data is **US-Government-Works** (public domain);
Fonteum composite terms apply to joined records (see the `_license` field on each
response).

## License

MIT. See [`LICENSE`](./LICENSE).

## Author & contact

Authored by **Dr. Jennifer Montecillo, MD**, medical reviewer, Fonteum.
Contact: `mcp@fonteum.com` · [fonteum.com](https://fonteum.com)
