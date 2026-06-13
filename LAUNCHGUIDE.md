# Fonteum — Healthcare Provider Data

## Tagline
Source-provenanced US healthcare provider data with an excluded-or-compromised-anywhere check.

## Description
Fonteum is a source-provenanced data layer for US healthcare providers and the organizations around them. It resolves any provider or facility by National Provider Identifier (NPI) or CMS Certification Number (CCN) across the major federal sources — the national provider registry, Medicare enrollment, quality data, and industry-payment records — and returns a joined record with the source, snapshot date, and license attached to every field. It determines whether a provider is excluded or otherwise compromised anywhere (federal OIG LEIE, federal SAM debarment, and state Medicaid exclusion lists, plus flags short of exclusion such as corporate integrity agreements and civil monetary penalties), traces the ownership chain behind a facility up to parent and private-equity entities, and exposes the published methodology behind every answer. It is for compliance, credentialing, payment-integrity, and due-diligence work that needs an audit-defensible provider fact tied back to its authoritative government source.

## Setup Requirements
- `FONTEUM_API_KEY` (optional): Defaults to the free read-only demo key `pk_dx_sample` (100 requests/hour/IP, no signup). Set a key for higher limits. https://fonteum.com
- `FONTEUM_API_BASE` (optional): Override the REST API base URL. Default: https://api.fonteum.com/v1
- Hosted endpoint requires no authentication; an optional `x-fonteum-mcp-key` header lifts the rate limit.

## Category
Data & Analytics

## Use Cases
Provider screening, Credentialing, Compliance audit, Payment integrity, Fraud research, Healthcare due diligence, Ownership transparency

## Features
- Resolve any provider or facility by NPI or CCN across federal sources, with per-field provenance
- Excluded-or-compromised-anywhere check across federal OIG LEIE, federal SAM, and state Medicaid exclusion lists
- Compromised-flag signals short of exclusion: corporate integrity agreements and civil monetary penalties
- Fourteen-field provenance contract on every field (source, snapshot date, methodology version, license, signed attestation reference)
- Facility ownership and private-equity chain lookup
- Provider search by specialty, location, or name
- Published methodology and source registry behind every answer
- FHIR R4, JSON REST, and MCP access surfaces

## Getting Started
- "List the federal healthcare data sources Fonteum reconciles, then look up NPI 1003000118."
- "Check whether NPI 1003000118 is excluded or compromised anywhere, with the source for each result."
- "Show the ownership chain behind hospital CCN 010001."

## Tags
healthcare, compliance, provider-data, npi, ccn, exclusion-screening, oig-leie, sam-gov, medicaid-exclusions, cms, open-payments, provenance, credentialing, fhir, ownership, payment-integrity

## Documentation URL
https://fonteum.com

## Health Check URL
https://fonteum.com/api/v1/health
