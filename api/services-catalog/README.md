# Published academic catalog

**`GET /api/mobile/v1/services/catalog`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{"departments":[],"programs":[],"courses":[],"prerequisites":[],"plans":[],"planItems":[]}`. Arrays contain the existing published/active academic catalog records.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON`, `500 INTERNAL_ERROR`.

## Notes

Removes any management-related response flag, exposes no catalog mutation routes.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
