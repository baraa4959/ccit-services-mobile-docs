# Read own per-activity attendance and review state

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/03-ATTENDANCE-AND-REVIEWS.md)

**`GET /api/mobile/v1/activities/{activityId}/attendance`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

UUID path, no query/body.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"registered":true,"reviewState":"PENDING","missingSteps":["EXIT"],"enteredAt":"<ISO8601>","exitedAt":null,"complete":false,"points":0}
```

## UI integration and business rules

`reviewState` may be `null|PENDING|APPROVED|REJECTED` based on current service; `missingSteps` contains ENTRY and/or EXIT, computed from configured attendance mode. Re-fetch after QR scan, review decision or uncertain network outcome. Submit review separately via `POST /activities/{activityId}/review`, with own GET and optional evidence endpoint.

## Errors

401 UNAUTHENTICATED, 403 STUDENT_ONLY, 404 NOT_FOUND, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
