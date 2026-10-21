# Approve mobile handoff — BROWSER ONLY

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../AUTHENTICATION.md)

**`POST /api/mobile/v1/auth/approve`** · **Auth:** browser Auth.js + same-origin; not native · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

HTML form `state` only; issued by `/mobile/connect` after browser sign-in; method POST same-origin.

**Example input** (placeholder credentials/IDs must be replaced):

```text
state=<pending-state>
```

## Success response

**HTTP 303** — example/wire shape:

HTTP 303 redirect to the configured HTTPS associated `/auth/callback?state=...&code=...`

## UI integration and business rules

**The mobile app must not invoke this API itself**; the web approval page owns it. The link contains one-use auth code, never a refresh/access token.

## Errors

401 UNAUTHENTICATED, 403 INVALID_ORIGIN, 400 INVALID_INPUT, 409 EXPIRED_OR_USED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
