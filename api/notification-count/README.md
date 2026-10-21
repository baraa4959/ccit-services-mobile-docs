# Unread count

**`GET /api/mobile/v1/me/notifications/unread-count`** · Bearer access token

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{"unreadCount":3}`.

## Errors

`401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
