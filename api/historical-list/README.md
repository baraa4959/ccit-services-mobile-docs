# Historical CCIT activities: student listing

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/activities/historical`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

No query or body.

## Success

200 `{"items":[{"id":"<uuid>","title":"...","description":"...","venue":"...","heldAt":"<ISO>","points":5,"participants":12,"participated":false,"reviewPending":false,"historical":true}]}`.

## Ownership, UI and business rules

Service mirrors `historicalActivityService.listOwn`: bounded newest 200 past CCIT records for an eligible student, including events without recorded participation so the student can request an attendance review. Do not call the staff-only historical list. Combine into Previous tab with ordinary completed/cancelled own activities.

## Errors

401 UNAUTHENTICATED; 403 STUDENT_ONLY; 500 INTERNAL_ERROR. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
