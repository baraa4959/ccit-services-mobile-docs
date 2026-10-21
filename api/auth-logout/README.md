# Sign out current device

**`POST /api/mobile/v1/auth/logout`** · Current refresh-token credential

[← API catalog](../README.md)

## Request

```json
{"refreshToken":"<session-uuid>.<current-refresh-secret>"}
```

## Success response

`200 OK`: `{"ok":true}`; then erase local secure credentials and device push registration.

## Errors

`400 INVALID_INPUT`, `401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`.

## Notes

Only the session matching the supplied current refresh secret is revoked. Do not rely on deleting client-side tokens alone.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
