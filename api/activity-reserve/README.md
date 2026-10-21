# Reserve an activity seat

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/02-ACTIVITIES.md)

**`POST /api/mobile/v1/activities/{activityId}/reserve`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

Valid UUID path; no JSON body required.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"ok":true}
```

## UI integration and business rules

Server checks eligibility, visible status, reservation-open and capacity; app refreshes detail/list after successful response. This is not automatic attendance or points. If reservation fails, retain original UI state and show backend code.

## Errors

401 UNAUTHENTICATED, 403 STUDENT_ONLY/ACTIVITY_NOT_ELIGIBLE, 404 NOT_FOUND, 409 CONFLICT/INVALID_STATE, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
