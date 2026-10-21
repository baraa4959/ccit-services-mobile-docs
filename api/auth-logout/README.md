# Sign out current mobile device

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../AUTHENTICATION.md)

**`POST /api/mobile/v1/auth/logout`** · **Auth:** refresh token · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

JSON `{refreshToken:string}`.

**Example input** (placeholder credentials/IDs must be replaced):

```json
{"refreshToken":"<current-session-id>.<current-refresh-secret>"}
```

## Success response

**HTTP 200** — example/wire shape:

```json
{"ok":true}
```

## UI integration and business rules

Backend revokes session and deletes associated device registrations; client also erases secrets and cached PII locally. Wrong/expired refresh may return 401; local sign-out still clears local device secrets.

## Errors

400 INVALID_INPUT, 401 UNAUTHENTICATED, 429 RATE_LIMITED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
