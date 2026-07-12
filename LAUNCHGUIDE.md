# Fonteum — Public Records & Sanctions

## Tagline

Source-traced public records for healthcare, procurement, sanctions,
enforcement, and corporate registers.

## Description

Fonteum is a read-only data layer for US healthcare, federal procurement,
sanctions and watchlists, federal enforcement, and global open-data corporate
registers. **111 active source families in the provenance ledger as of
2026-07-12.** Results identify the available official source, the date Fonteum
captured the record, and relevant coverage or limitation details.

Use stable identifiers such as NPI, UEI, and CAGE to resolve records; search
US healthcare records by attribute; and check applicable exclusion, debarment,
and sanctions lists by NPI or name. For a material match, consult the issuing
authority before acting. Fonteum returns dated source facts, not a clearance,
risk score, or verdict.

## Connect

- Hosted MCP endpoint: `https://fonteum.com/api/mcp`
- Local stdio package: `npx -y @fonteum/mcp@0.3.0`
- Product documentation: [fonteum.com](https://fonteum.com)

## Category

Data & Analytics

## Use cases

Public-record research, procurement research, sanctions and watchlist review,
source-traceable reporting, program-integrity research, and corporate-register
research.

## Tools

The hosted MCP server and `@fonteum/mcp` use the same seven read-only tools:

- `fonteum_resolve_entity` — resolve an entity by NPI, UEI, or CAGE.
- `fonteum_search_records` — search US healthcare records by attribute.
- `fonteum_check_exclusions_and_sanctions` — check an NPI or name against
  applicable exclusion, debarment, and sanctions lists.
- `fonteum_get_record_as_of` — return a federal contractor record as captured
  on a supplied date.
- `fonteum_recheck` — return source and capture information for a follow-up
  check.
- `fonteum_list_sources` — list active source families and their official
  sources.
- `fonteum_dataset_info` — return the current methodology, scope, and
  source-catalog metadata.

## Source context

Record availability varies by source family and jurisdiction. An absent record
is not a determination about a person or organization. The REST, OpenAPI,
hosted MCP, and npm interfaces describe the same public-records product
contract.

## Tags

public-records, sanctions, watchlists, healthcare, federal-procurement,
federal-enforcement, corporate-registers, open-data, source-traceability

## Maintainer and medical review

Fonteum LLC maintains this service. Medical review: Dr. Jennifer Montecillo,
MD.
