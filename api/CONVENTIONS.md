# API conventions

[← Catalog](README.md) · [Authentication](AUTHENTICATION.md)

**Base URL:** `https://ccitstudents.com/api/mobile/v1` (use separately provided Staging URL for QA). All protected calls supply `Authorization: Bearer <accessToken>`. JSON requests use `Content-Type: application/json`; reviews use `multipart/form-data` with a native boundary. Do **not** manually set multipart Content-Type without its boundary. Binary success for reports/evidence requires binary handling; errors are JSON `{ "error":"CODE" }`. No generic success `data` wrapper. No-store responses; never log tokens, personal evidence, or signed QR contents.

| HTTP | Typical code | Behavior |
|---|---|---|
| 400 | INVALID_INPUT, INVALID_FILTER, INVALID_FORMAT, INVALID_FILE | Fix validation; do not retry blindly |
| 401 | UNAUTHENTICATED, SESSION_EXPIRED | Serialized refresh or restart OS-browser sign-in |
| 403 | STUDENT_ONLY, FORBIDDEN, OUTSIDE_GEOFENCE, RESERVATION_REQUIRED | Show relevant Arabic message; do not bypass |
| 404 | NOT_FOUND, CERTIFICATE_NOT_ELIGIBLE | Not eligible or not owned |
| 409 | CONFLICT, INVALID_STATE, MISSING_REPORT_FIELDS | Re-fetch state; report missing fields if applicable |
| 413 | FILE_TOO_LARGE | File/request size exceeds published limit |
| 429 | RATE_LIMITED | Backoff; no offline QR replay |
| 500/503 | INTERNAL_ERROR, UNAVAILABLE | Server/operational issue, preserve user input |

**Naming:** Mobile-specific data camelCase; `definition` is the underlying typed activity object; historical routes convert own list/detail to camelCase. UUIDs are opaque. Timestamps are ISO strings in JSON serialization. Ordinary activity history cap 200 and historical list cap 200, points ledger cap 200, notifications cursor-paginated. No `userId` parameter on own-only routes. Staff-only accounts are not entitled to student APIs. `SERVICE_COMING_SOON` remains a web policy for unpublished modules, not a successful response to one of the four released services.
