# API Authentication & Security

[← Endpoint catalog](README.md) · [API conventions](CONVENTIONS.md)

## Authentication and general contract

Native authenticated routes require `Authorization: Bearer <accessToken>` with **no cookie**. `401 UNAUTHENTICATED` means refresh once (serialized), retry, then return to browser sign-in if refresh fails. Access tokens expire in 900 seconds; refresh tokens rotate, and the inactivity window is 90 days. These strings are sensitive and must be kept in OS secure storage (never source control, logs, analytics, or URLs). No static API secret belongs inside the app. Browser approval is the only cookie-bound route and requires the normal same-origin CSRF check.

All responses use JSON with `Cache-Control: no-store` (except browser redirects), all timestamps are ISO-8601 UTC, identifiers are opaque UUIDs, and errors are `{ "error": "STABLE_CODE" }`. No mobile endpoint takes `userId` as an ownership parameter. Scope and permission data is informative only; server authorizes each request.

## First sign-in: verified browser handoff

1. The app generates a random high-entropy `codeVerifier` (43–128 RFC 7636 unreserved characters) and `codeChallenge = base64url(SHA256(ASCII(codeVerifier)))`, **no padding**. Call [`POST auth/start`](auth-start/README.md). Do not send the verifier yet.
2. Save the returned `state`, then open `authorizationUrl` in the **system browser/authentication session**. Web performs its existing password/passkey/Microsoft/2FA and account activation flow. After authentication, a browser page asks the user to approve the explicit mobile handoff. This form calls [`POST auth/approve`](auth-approve/README.md), a web-only same-origin action.
3. The server redirects to the **fixed, deployment-configured HTTPS Universal/App Link** `MOBILE_APP_CALLBACK_URL?state=...&code=...`. The app must verify it handled an actual associated domain link and verify returned `state` matches the saved outstanding request; reject unsolicited or duplicate callbacks. There is no access/refresh token in the link.
4. Call [`POST auth/exchange`](auth-exchange/README.md) with `state`, `code`, `codeVerifier`. Save returned access/refresh pair securely as one transaction. Request [`GET /bootstrap`](bootstrap/README.md) for the latest identity/services. Clear outstanding verifier/state/code from memory. The handoff request expires after 15 minutes; the one-use code must be redeemed within two minutes of browser approval.

## Every protected API call

```http
GET /api/mobile/v1/me/profile HTTP/1.1
Authorization: Bearer <current-access-token>
Accept: application/json
```

A valid bearer is checked against the active server-side mobile session, 15-minute access expiry and the **current active account status**. No browser cookie or `Origin` header is used to grant mobile bearer access. Staff permissions are derived from **currently active role assignments and their own scopes**; UI hints do not authorize backend actions.

If access expires: serialize a single [`auth/refresh`](auth-refresh/README.md) per session; update both tokens atomically in OS secure storage; retry the original request once. The rotating refresh token is not a perpetual password: a known previously rotated token replay revokes the session. The 90-day expiry is **sliding inactivity**, moved forward by an authenticated API request or refresh, not by server-to-device push. A `401` refresh failure requires a full browser login. Web sign-out does not necessarily revoke a separately issued Mobile session; server account deactivation/session revocation does.

Biometrics (Face ID/Touch ID/device unlock) only gate retrieval of already-issued credentials on the device; biometric results are not sent as evidence of server identity. Use iOS Keychain / Android Keystore-backed secure storage. No static secret in a reverse-engineerable app. TLS required throughout.

## Error reference

| HTTP | Typical code | Client response |
|---|---|---|
| 400 | `INVALID_INPUT`, `INVALID_CURSOR` | Fix request/QR location payload or cursor. |
| 401 | `UNAUTHENTICATED`, `SESSION_EXPIRED`, `INVALID_OR_EXPIRED_CODE` | Refresh once or begin browser login; never reuse a redeemed code. |
| 403 | `STUDENT_ONLY`, `FORBIDDEN`, `INVALID_ORIGIN` | Hide unsupported operation; web form cannot be called as a native API. |
| 403 | `OUTSIDE_GEOFENCE`, `ACTIVITY_NOT_ELIGIBLE`, `RESERVATION_REQUIRED`, `STUDENT_SETUP_REQUIRED` | Show actionable, localized attendance feedback. |
| 404 | `NOT_FOUND` | Not available to this authenticated user. |
| 409 | `INVALID_STATE`, `CONFLICT`, `EXPIRED_OR_USED` | Refresh state, check duplicate submission or restart sign-in. |
| 429 | `RATE_LIMITED` | Back off; do not auto-retry scans in a tight loop. |
| 500/503 | `INTERNAL_ERROR`, `UNAVAILABLE` | Present a temporary-service error; do not assume a mutation failed if the response was lost. |

## API authentication tests to run before release

Wrong state, wrong verifier, reused code, 15-minute handoff and two-minute code expiry, CSRF/browser-origin mismatch, expired/revoked source web session, bearer invalid, access expiration, refresh rotation and replay, 90-day idle expiry, revoked and non-active user, role/scope change, object ownership, parallel refresh, and production student beta gate. Coordinate these with the project owner using an authorized integration-test environment; local app mocks alone do not validate them.
