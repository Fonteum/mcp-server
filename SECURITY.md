# Security Policy

## Service model

Fonteum is a read-only public-records and enforcement-integrity service. It
covers US healthcare, federal procurement, sanctions and watchlists, federal
enforcement, intellectual property, securities filings, and supported global
open-data company registers.

The hosted endpoint is `https://fonteum.com/api/mcp`. This repository does
not publish a sample credential or require one in its connection examples.
Clients must protect any credentials issued to them and must not commit them to
source control, logs, or MCP configuration that is shared publicly.

The service exposes the read-only tools described in the [README](./README.md):

- `fonteum_resolve_entity`
- `fonteum_search_records`
- `fonteum_check_exclusions_and_sanctions`
- `fonteum_get_record_as_of`
- `fonteum_recheck`
- `fonteum_list_sources`
- `fonteum_dataset_info`

## Rate limiting

Operational limits may change. Clients should handle rate-limit responses with
bounded retries and backoff. The service does not rely on client-supplied
forwarding headers to establish a caller's identity.

## Input validation

Tool inputs are validated before a lookup. Stable identifiers use their
documented formats (including NPI, UEI, and CAGE); date and search inputs are
validated by the relevant tool.

## Network access and data handling

The service reads named public-record source families and returns source context
for the records it serves. It does not provide a risk score, clearance, or
verdict. Re-confirm a material match with the issuing authority before acting
on it.

## Maintainer and medical review

Fonteum LLC maintains this service. Medical review: Dr. Jennifer Montecillo,
MD.

## Responsible disclosure

Report security concerns to **security@fonteum.com**. We aim to acknowledge
reports within 3 business days.
