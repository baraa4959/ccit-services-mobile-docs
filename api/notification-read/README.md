# Mark notification as read

**`PATCH /api/mobile/v1/me/notifications/{notificationId}/read`** · Bearer access token

[← API catalog](../README.md)

## Request

Empty body; `{notificationId}` must be a UUID.

## Success response

`200 OK`: `{"ok":true}`. Repeat calls are idempotent.

## Errors

`400 INVALID_INPUT`, `401 UNAUTHENTICATED`, `404 NOT_FOUND`, `500 INTERNAL_ERROR`.

## Notes

Object-level authorization: a notification owned by another user returns `404` and is not altered.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
