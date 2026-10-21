# Reserve activity

**`POST /api/mobile/v1/activities/{activityId}/reserve`** · Bearer; active student

[← API catalog](../README.md)

## Request

Empty body; `{activityId}` is a UUID.

## Success response

`200 OK`: `{"ok":true}`.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `ACTIVITY_NOT_ELIGIBLE` / `FORBIDDEN` / `SERVICE_COMING_SOON`, `404 NOT_FOUND`, `409 CONFLICT` / `INVALID_STATE` / `RESERVATION_REQUIRED`.

## Notes

Uses existing server reservation/capacity/business logic. Do not infer entitlement from UI alone.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
