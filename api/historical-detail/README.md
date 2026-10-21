# Historical activity: own student detail

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/activities/historical/{id}`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

`id` UUID, no body.

## Success

200 `{"activity":{"id":"<uuid>","title":"...","description":null,"venue":"...","heldAt":"<ISO>","points":5,"participants":12,"historical":true},"participated":false,"reviewState":null,"canRequestReview":true,"reviewCategory":"ATTENDANCE","certificateEligible":false}`.

## Ownership, UI and business rules

If already participated, `reviewCategory` is POINTS. Only one review is permitted even after decision. Certificate eligibility uses actual server point/participation checks. No manager actions or roster returned.

## Errors

401 UNAUTHENTICATED; 403 STUDENT_ONLY; 404 NOT_FOUND. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
