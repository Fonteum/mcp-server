# Fonteum — Public Records & Enforcement Integrity

## Tagline

Source-linked public records for healthcare, procurement, sanctions,
enforcement, and global company registers.

## Description

Fonteum is a read-only public-records and enforcement-integrity data platform
for US healthcare, federal procurement, sanctions and watchlists, federal
enforcement, intellectual property, securities filings, and supported global
open-data company registers. Results identify the official source, the date
Fonteum captured the record when available, and relevant coverage or limitation
details.

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

The hosted MCP server exposes these read-only tools:

- `fonteum_resolve_entity` — find records for one NPI, UEI, or CAGE identifier.
- `fonteum_search_records` — find matching healthcare-provider records by
  provider type and state.
- `fonteum_check_exclusions_and_sanctions` — show supported list entries for
  an NPI or name.
- `fonteum_get_record_as_of` — show available contractor records for a supplied
  identifier and past date.
- `fonteum_recheck` — inspect Fonteum's stored verification record or a
  specified snapshot.
- `fonteum_list_sources` — show the current public source catalog.
- `fonteum_dataset_info` — explain current method, coverage notes, and how to
  read source details.

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
