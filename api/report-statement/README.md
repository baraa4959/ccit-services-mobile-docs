# Export own Activity Points statement

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/reports/statement`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

Query `format=pdf|xlsx` (default `pdf`). No userId or ids.

## Success

200 binary PDF (`application/pdf`) or Excel workbook (`application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`), `Content-Disposition: attachment`, no-store.

## Ownership, UI and business rules

Uses existing `pointStatement(actor.userId)` and audited export; same own ledger as web. Requires number, department and program for PDF/XLSX consistency. Treat binary as a file, not JSON.

## Errors

400 INVALID_INPUT/INVALID_FORMAT; 403 STUDENT_ONLY/FORBIDDEN; 409 MISSING_REPORT_FIELDS; 413 TOO_MANY_TRANSACTIONS; 500 INTERNAL_ERROR. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
