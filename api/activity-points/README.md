# Read own total activity points

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/04-POINTS.md)

**`GET /api/mobile/v1/activities/points`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

No body/query.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"balance":15,"earned":20,"deducted":5}
```

## UI integration and business rules

Totals aggregate multiple ledgers (ordinary activities, historical activity credits and adjustments, separate admin adjustments). Numbers illustrative. Do not derive total from last 200 displayed history rows.

## Errors

401 UNAUTHENTICATED, 403 STUDENT_ONLY/FORBIDDEN, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
