# Read own recent point transactions

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/04-POINTS.md)

**`GET /api/mobile/v1/activities/points/history`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

No query/body; history cap currently 200; no cursor.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"items":[{"id":"<uuid>","activityId":"<uuid>","title":"Example activity","delta":5,"reason":"Attendance points","createdAt":"<ISO8601>","historical":false,"administrative":false}]}
```

## UI integration and business rules

`delta>0` earned, `<0` deducted. Historical and admin flags affect destination routing. All/earned/deducted filters are client side. Do not invent older-history pagination that current route does not support.

## Errors

401 UNAUTHENTICATED, 403 STUDENT_ONLY/FORBIDDEN, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
