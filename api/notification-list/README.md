# List own notification inbox with cursor

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/09-NOTIFICATIONS.md)

**`GET /api/mobile/v1/me/notifications`** · **Auth:** bearer · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

Optional `limit` 1–50 default 30 and `cursor` opaque Base64url 1–512 chars from prior `nextCursor`. No body/userId.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"items":[{"id":"<uuid>","category":"ACTIVITY","title":"Example","message":"Example notification","href":"/app/activities/<uuid>","readAt":null,"createdAt":"<ISO8601>"}],"nextCursor":null}
```

## UI integration and business rules

`href` nullable and filtered through safe internal-link check; never directly open arbitrary URL. Keep fetching while nextCursor non-null. UI reconciles notifications by ID. A malformed/expired cursor returns INVALID_CURSOR.

## Errors

400 INVALID_INPUT/INVALID_CURSOR, 401 UNAUTHENTICATED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
