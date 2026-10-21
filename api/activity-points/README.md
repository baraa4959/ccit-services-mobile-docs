# Own points balance

**`GET /api/mobile/v1/activities/points`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{"balance":25,"earned":30,"deducted":5}` (illustrative numbers only). All values calculated by the authoritative activity ledger, including eligible historical/administrative adjustments.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `STUDENT_SETUP_REQUIRED` / `FORBIDDEN` / `SERVICE_COMING_SOON`, `500 INTERNAL_ERROR`.

## Notes

Staff-only clients cannot query student points.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
