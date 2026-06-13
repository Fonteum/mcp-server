# Security Policy

## Authentication model
- **Hosted endpoint** (`https://mcp.fonteum.com/api/mcp`): no authentication required. Anonymous access is rate-limited per client IP. An optional `x-fonteum-mcp-key` header raises the limit.
- **Local / npm** (`@fonteum/mcp`): defaults to the free read-only demo key `pk_dx_sample` (no signup). Set `FONTEUM_API_KEY` for higher limits.
- The server is **read-only**. It exposes no mutation tools and never writes to upstream sources.

## Rate limiting
- Limits are enforced per **trusted client identity** — the platform-provided source IP plus the API key — not on any client-supplied forwarding header.
- A spoofed or forged `X-Forwarded-For` (or similar) header does **not** reset or evade the limit.
- Current policy: anonymous hosted access ~30 requests/minute/IP; demo-key REST access ~100 requests/hour/IP. Keys lift these limits.

## Input validation
- Every public parameter is constrained and rejected with a `400` if malformed:
  - **NPI**: exactly 10 digits, Luhn-checked.
  - **CCN**: validated against the CMS Certification Number format.
  - All enum/range parameters (state, vertical, dataset, limits) are bounded to documented allowed values.
- Constraints are published in the OpenAPI 3.1 specification.

## Network access
- The server makes **read-only outbound requests only** to named, public government data sources: CMS NPPES, OIG LEIE, GSA SAM.gov, state Medicaid exclusion lists, CMS PECOS, CMS Care Compare, CMS Open Payments, CMS ownership datasets, and GLEIF. It performs no other network egress.
- All underlying data is public-domain federal or state government data. The server stores and returns no end-user personal data beyond already-public provider identifiers.

## Responsible disclosure
Report security concerns to **security@fonteum.com**. We aim to acknowledge within 3 business days.
