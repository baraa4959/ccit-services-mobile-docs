# Withdraw own reservation

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/02-ACTIVITIES.md)

**`POST /api/mobile/v1/activities/{activityId}/withdraw`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

Valid UUID path; no JSON body required.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"ok":true}
```

## UI integration and business rules

Use only for currently reserved activity when allowed. After success refetch list/detail; withdrawal must not retroactively erase already recorded attendance/points.

## Errors

401 UNAUTHENTICATED, 403 STUDENT_ONLY, 404 NOT_FOUND, 409 CONFLICT/INVALID_STATE, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
