# Exchange one-time code for mobile credentials

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../AUTHENTICATION.md)

**`POST /api/mobile/v1/auth/exchange`** · **Auth:** one-use code + PKCE · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

JSON `state` (43), `code` (43), `codeVerifier` (43–128 RFC7636 unreserved chars).

**Example input** (placeholder credentials/IDs must be replaced):

```json
{"state":"<original-state>","code":"<callback-code>","codeVerifier":"<original-verifier>"}
```

## Success response

**HTTP 200** — example/wire shape:

```json
{"accessToken":"<session-uuid>.<secret>","refreshToken":"<session-uuid>.<secret>","tokenType":"Bearer","expiresIn":900,"session":{"id":"<uuid>","expiresAt":"<ISO8601>","inactivityTimeoutDays":90},"user":{"id":"<uuid>","displayName":"...","identifiers":[],"affiliations":["STUDENT"],"roles":[],"permissions":[],"isStudent":true,"isStaff":false,"mobileStaffSupported":false}}
```

## UI integration and business rules

`user` may be null if a separately retryable bootstrap fetch fails; tokens remain valid. Compare callback state before exchange. One-use code expires 2 minutes after approval; browser request 15 minutes. Persist issued pair atomically.

## Errors

400 INVALID_INPUT, 401 INVALID_OR_EXPIRED_CODE, 429 RATE_LIMITED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
