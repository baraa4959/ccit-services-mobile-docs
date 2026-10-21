# List student activities

**`GET /api/mobile/v1/activities`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{"isStudent":true,"activities":[{"id":"<uuid>","collegeRef":"ccit","status":"PUBLISHED","definition":{...},"displayStatus":"...","reserved":false,"participated":false,"studentEligible":true,...}]}`. Eligibility/reservation/participation is calculated on server; list limits match existing 200-row activity service.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON`, `500 INTERNAL_ERROR`.

## Notes

Only student-eligible, reserved or participated activities are returned. Staff/manage-only list items are removed.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
