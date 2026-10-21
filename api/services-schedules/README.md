# Published schedules

**`GET /api/mobile/v1/services/schedules`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{"terms":[<published term objects>],"sections":[<published section objects>],"meetings":[<section meeting objects>]}`. Published visibility and school business rules reuse `campusWorkflows.schedules`.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON`, `500 INTERNAL_ERROR`.

## Notes

Mobile intentionally omits the web editor flag and offers read-only schedule data.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
