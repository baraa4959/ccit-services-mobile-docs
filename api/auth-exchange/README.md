# Exchange sign-in code

**`POST /api/mobile/v1/auth/exchange`** · Public (one-time code + PKCE proof)

[← API catalog](../README.md)

## Request

```json
{"state":"<returned-state>","code":"<app-link-code>","codeVerifier":"<original-random-verifier>"}
```

## Success response

`200 OK`
```json
{"accessToken":"<session-uuid>.<secret>","refreshToken":"<session-uuid>.<secret>","tokenType":"Bearer","expiresIn":900,"session":{"id":"<uuid>","expiresAt":"<ISO-8601>","inactivityTimeoutDays":90},"user":{"id":"<uuid>","displayName":"...","accountStatus":"ACTIVE","identifiers":[],"affiliations":["STUDENT"],"roles":[],"permissions":[],"isStudent":true,"isStaff":false,"mobileStaffSupported":false}}
```
`user` can be `null` if the separately retryable bootstrap lookup fails **after** tokens were safely issued; call `/bootstrap` with the access token in that case.

## Errors

`400 INVALID_INPUT`, `401 INVALID_OR_EXPIRED_CODE`, `500 INTERNAL_ERROR`.

## Notes

Checks matching state, SHA-256 PKCE, unused code, active account and source web session under a DB transaction. No client-controlled user ID or redirect URI.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
