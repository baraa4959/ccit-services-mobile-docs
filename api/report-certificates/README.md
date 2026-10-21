# Export eligible own certificates

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/reports/certificates`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

Query `ids=<uuid>[,<uuid>...]` (1–20 distinct) and optional `format=pdf` (the only supported format). All IDs must be valid UUIDs.

## Success

200 PDF binary with certificates for exactly the requested, eligible events. Header `Content-Disposition: attachment; filename=ccit-certificates.pdf`.

## Ownership, UI and business rules

Each historical activity requires existing own certificate eligibility; each ordinary activity requires completed own attendance and positive points. No staff certificate creation or another student ID.

## Errors

400 INVALID_FILTER/INVALID_FORMAT; 401 UNAUTHENTICATED; 403 STUDENT_ONLY; 404 CERTIFICATE_NOT_ELIGIBLE; 500 INTERNAL_ERROR. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
