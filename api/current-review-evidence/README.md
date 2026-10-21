# Current activity: download own review evidence

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/activities/{activityId}/review/{reviewId}/evidence`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

Both IDs are UUIDs. No body. Use returned `review.id` from own GET.

## Success

200 binary with Content-Type `image/png`, `image/jpeg` or `application/pdf`; Content-Disposition attachment, no-store/nosniff.

## Ownership, UI and business rules

Server verifies review owner (or manager in underlying service, but mobile route requires STUDENT identity); guessing an ID cannot grant another person’s file. Never inline untrusted file in a web view.

## Errors

401 UNAUTHENTICATED; 403 STUDENT_ONLY/FORBIDDEN; 404 NOT_FOUND. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
