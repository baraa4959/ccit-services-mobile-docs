# Historical activity: read or request own attendance/points review

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET / POST /api/mobile/v1/activities/historical/{id}/reviews`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

`id` UUID. GET no body. POST multipart with `category=ATTENDANCE|POINTS`, `reason` trimmed 5–1500 chars, optional `evidence` JPEG/PNG/PDF ≤2,000,000 bytes (whole request ≤2,500,000).

## Success

GET 200: `{"review":null}` or `{"review":{"id":"<uuid>","category":"ATTENDANCE","reason":"...","state":"PENDING","resolution":null,"createdAt":"<ISO>","hasEvidence":true}}`. POST 201: `{"ok":true}`.

## Ownership, UI and business rules

Server verifies active college student and past event; ATTENDANCE only if not participated; POINTS only if already participated. Unique one per event/user. Approval decisions are staff-only in WEB. Approved POINTS does not silently adjust points.

## Errors

400 INVALID_INPUT/INVALID_FILE; 401 UNAUTHENTICATED; 403 STUDENT_ONLY/FORBIDDEN; 404 NOT_FOUND; 409 INVALID_STATE/CONFLICT; 413 FILE_TOO_LARGE. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
