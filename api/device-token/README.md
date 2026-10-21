# Register or delete a native push device

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/09-NOTIFICATIONS.md)

**`POST/DELETE /api/mobile/v1/devices/push-token`** · **Auth:** bearer · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

POST strict JSON `{platform:"IOS"|"ANDROID",pushToken:string (16–4096 chars)}` max 5000-char JSON; DELETE has no body and removes current session devices.

**Example input** (placeholder credentials/IDs must be replaced):

```json
{"platform":"ANDROID","pushToken":"<native-FCM-registration-token>"}
```

## Success response

**HTTP 200** — example/wire shape:

```json
{"ok":true}
```

## UI integration and business rules

iOS submits raw **APNs device token**, Android native **FCM registration token**; never Expo token. A provider token is reassigned to one user/session, and ciphertext is stored encrypted. Register after login/permission/token rotation; DELETE on deregistration, logout also removes associated tokens. Backend provider credentials are never placed in mobile app.

## Errors

400 INVALID_INPUT, 401 UNAUTHENTICATED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
