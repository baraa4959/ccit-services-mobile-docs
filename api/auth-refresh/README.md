# Rotate session credentials

**`POST /api/mobile/v1/auth/refresh`** · Refresh-token credential (no Cookie)

[← API catalog](../README.md)

## Request

```json
{"refreshToken":"<session-uuid>.<current-refresh-secret>"}
```

## Success response

`200 OK`: same token/`expiresIn`/`session` shape as exchange (without `user`). Both access and refresh secrets are replaced; save the pair atomically and serialize concurrent refreshes.

## Errors

`400 INVALID_INPUT`, `401 SESSION_EXPIRED`, `500 INTERNAL_ERROR`.

## Notes

Known previously rotated refresh-token replay revokes that mobile session. Arbitrary guessed token hashes do not revoke it. A valid refresh extends the 90-day idle deadline.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
