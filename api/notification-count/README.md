# Read own unread badge count

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/09-NOTIFICATIONS.md)

**`GET /api/mobile/v1/me/notifications/unread-count`** · **Auth:** bearer · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

No body/query.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"unreadCount":3}
```

## UI integration and business rules

Number illustrative. Refresh on app open/resume, inbox read operation, and incoming/tapped push; this does not mark notifications read.

## Errors

401 UNAUTHENTICATED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
