# Own student service requests

**`GET /api/mobile/v1/services/requests`** · Bearer; active student and existing service grant

[← API catalog](../README.md)

## Query / path parameters

Required query `kind`: `ADD_COURSE` | `DROP_COURSE` | `CHANGE_SECTION` | `VISITOR_OUT` | `TRAINING`. Incoming external visitor (`VISITOR_IN`) is not supported by this endpoint.

## Request

No body.

## Success response

`200 OK`: `{"requests":[<existing CCIT service case records>]}`. Cases are current DB records owned by the signed-in student, up to 100 newest-first; fields include `id`, `kind`, `status`, `created_at`/`updated_at` from the existing schema.

## Errors

`400 INVALID_INPUT`, `401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON` / `FORBIDDEN`, `500 INTERNAL_ERROR`.

## Notes

No review/staff mode is exposed.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
