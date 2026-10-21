# Rotate mobile session credentials

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../AUTHENTICATION.md)

**`POST /api/mobile/v1/auth/refresh`** · **Auth:** refresh token · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

JSON `{refreshToken:"<session-uuid>.<secret>"}`.

**Example input** (placeholder credentials/IDs must be replaced):

```json
{"refreshToken":"<current-session-id>.<current-refresh-secret>"}
```

## Success response

**HTTP 200** — example/wire shape:

```json
{"accessToken":"<rotated>","refreshToken":"<rotated>","tokenType":"Bearer","expiresIn":900,"session":{"id":"<uuid>","expiresAt":"<ISO8601>","inactivityTimeoutDays":90}}
```

## UI integration and business rules

Serialize concurrent refresh attempts; store both new tokens as a pair. Known replay of previous refresh secret revokes session. An API-authenticated request/refresh slides idle expiry; push delivery does not.

## Errors

400 INVALID_INPUT, 401 SESSION_EXPIRED, 429 RATE_LIMITED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
