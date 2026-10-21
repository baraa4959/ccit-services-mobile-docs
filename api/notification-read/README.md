# Mark one own notification read

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/09-NOTIFICATIONS.md)

**`PATCH /api/mobile/v1/me/notifications/{notificationId}/read`** · **Auth:** bearer · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

UUID `{notificationId}` path, no body.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"ok":true}
```

## UI integration and business rules

Recipient-scoped and idempotent read timestamp; mark on row open before in-app navigation. Not-own/missing UUID → 404, malformed ID → 400. Re-fetch badge.

## Errors

400 INVALID_INPUT, 401 UNAUTHENTICATED, 404 NOT_FOUND, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
