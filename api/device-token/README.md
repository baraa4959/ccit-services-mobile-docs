# Register or remove a native push device

**`POST / DELETE /api/mobile/v1/devices/push-token`** · Bearer access token; authenticated user (including staff-only accounts for account notifications)

[← API catalog](../README.md) · [Push implementation guide](../../04-PUSH-NOTIFICATIONS.md)

## Register device — POST

After the user is authenticated and the platform returns a current native token, send:

```http
POST /api/mobile/v1/devices/push-token HTTP/1.1
Authorization: Bearer <access-token>
Content-Type: application/json

{"platform":"IOS","pushToken":"<native-apns-device-token>"}
```

Android uses `"platform":"ANDROID"` and the **FCM registration token**. Allowed platform values: `IOS` / `ANDROID`. Token length: 16–4096 characters. Do not submit an Expo push token, Firebase service-account credential, Apple `.p8` key, or a user login token as `pushToken`.

**Success:** HTTP `200` with `{"ok":true}`. Registration binds the provider token to the currently authenticated mobile session. Re-register if the OS/provider rotates the token, on a newly signed-in session, and when the app can detect a changed token. If notification permission is denied or the token is unavailable, keep the in-app inbox functional and retry token registration when the user enables notifications and a token is available.

## Unregister current session — DELETE

```http
DELETE /api/mobile/v1/devices/push-token HTTP/1.1
Authorization: Bearer <access-token>
```

No JSON body. **Success:** HTTP `200` with `{"ok":true}`. Deletes push registrations belonging to the current session. Call during sign-out while bearer access is valid, then call the documented auth logout flow and erase local credentials. Server session revocation also limits the session's access; do not claim sign-out is complete merely by deleting a local token.

## Errors

`400 INVALID_INPUT`, `401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`. Use the usual serialized refresh logic on expired access tokens. Never log full provider tokens.

## Provider payload and client behavior

The current CCIT worker sends a generic visible notification and an opaque `notificationId`: iOS receives `notificationId` as a custom APNs payload field; Android receives it in the FCM `data` map. Parse and validate the ID before navigating; fetch the corresponding current item from the authenticated [notifications API](../notification-list/README.md) and display only accessible data. Push delivery is supplementary: the inbox is authoritative. Handle foreground, background, tapped and terminated-app launches with platform-appropriate notification handlers and deduplicate repeats by notification ID. APNs sandbox/production must match the installed app's signing environment. Test on real devices with provider accounts configured.

See [04 — Push notifications](../../04-PUSH-NOTIFICATIONS.md) for the end-to-end client checklist.
