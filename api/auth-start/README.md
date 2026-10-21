# Start web authentication

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../AUTHENTICATION.md)

**`POST /api/mobile/v1/auth/start`** · **Auth:** public · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

JSON `{codeChallenge:string (43 base64url chars), codeChallengeMethod:"S256"}`; challenge = SHA256(verifier) base64url without padding.

**Example input** (placeholder credentials/IDs must be replaced):

```json
{"codeChallenge":"<43-character-S256-challenge>","codeChallengeMethod":"S256"}
```

## Success response

**HTTP 201** — example/wire shape:

```json
{"state":"<43-character-state>","authorizationUrl":"https://ccitstudents.com/mobile/connect?state=<state>","expiresIn":900}
```

## UI integration and business rules

Save state and verifier for the outstanding attempt and open `authorizationUrl` through the OS-managed browser. Request expires 15 min. Verifier must not be sent here.

## Errors

400 INVALID_INPUT, 429 RATE_LIMITED, 503 UNAVAILABLE. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
