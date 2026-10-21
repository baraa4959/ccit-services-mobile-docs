# Current activity: read or submit own attendance review

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET / POST /api/mobile/v1/activities/{activityId}/review`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

`{activityId}` valid UUID. GET no body. POST `multipart/form-data`: `reason` (trimmed 5–1500 chars), optional `evidence` JPG/PNG/PDF ≤2,000,000 bytes; total multipart request ≤2,500,000 bytes. Let the HTTP client generate the multipart boundary. Example fields: `reason=I missed the entry` and `evidence=@photo.jpg`.

## Success

GET 200: `{"review":null}` or `{"review":{"id":"<uuid>","reason":"...","state":"PENDING","resolution":null,"createdAt":"<ISO>","hasEvidence":true}}`. POST 201: `{"ok":true}`.

## Ownership, UI and business rules

The server checks active CCIT student profile, ordinary activity ownership/eligibility, reservation/attendance and missing step; request permitted while event is running when a step is reviewable, unique one per student/activity. Submission does not grant attendance/points. This GET shows only the current user’s review, never a staff review queue.

## Errors

400 INVALID_INPUT/INVALID_FILE; 401 UNAUTHENTICATED; 403 STUDENT_ONLY/FORBIDDEN; 404 NOT_FOUND; 409 INVALID_STATE/CONFLICT; 413 FILE_TOO_LARGE. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
