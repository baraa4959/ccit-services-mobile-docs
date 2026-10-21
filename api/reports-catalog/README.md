# Available student-only reports

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/services/reports`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

No query or body.

## Success

200 `{"items":[{"id":"POINTS_OWN","titleAr":"كشف رصيدي","formats":["pdf","xlsx"],"path":"/api/mobile/v1/reports/statement"},{"id":"CERTIFICATES","titleAr":"شهادات الأنشطة","formats":["pdf"],"path":"/api/mobile/v1/reports/certificates"}]}`.

## Ownership, UI and business rules

No manager report options, other user ID selectors or event-wide exports.

## Errors

401 UNAUTHENTICATED; 403 STUDENT_ONLY. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
