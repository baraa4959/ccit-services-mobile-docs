# Historical activity: fetch own review evidence

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/activities/historical/{id}/reviews/{reviewId}/evidence`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

Both IDs valid UUID; no body. Use review ID from own review GET.

## Success

200 binary and Content-Type for JPEG/PNG/PDF, `Cache-Control: private, no-store`, `X-Content-Type-Options: nosniff`, download attachment.

## Ownership, UI and business rules

Underlying service checks user ownership; there is no public evidence listing and no manager approval API in mobile.

## Errors

401 UNAUTHENTICATED; 403 STUDENT_ONLY/FORBIDDEN; 404 NOT_FOUND. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
