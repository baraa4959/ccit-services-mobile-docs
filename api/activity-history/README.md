# Own points history

**`GET /api/mobile/v1/activities/points/history`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{ "items": [<existing point ledger/history rows>] }`. Entries include activity/adjustment entries in the current authoritative activity history; do not manufacture totals in the app.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `FORBIDDEN` / `SERVICE_COMING_SOON`, `500 INTERNAL_ERROR`.

## Notes

This is the history counterpart to the aggregate balance endpoint.

See [API authentication](../AUTHENTICATION.md).
