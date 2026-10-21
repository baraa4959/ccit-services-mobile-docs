# Read activity detail

**`GET /api/mobile/v1/activities/{activityId}`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body; `{activityId}` is a UUID.

## Success response

`200 OK`: existing public activity representation including `id`, `collegeRef`, `status`, `definition`, `createdAt`, `updatedAt`, `displayStatus`, `reserved`; *no staff editing flags*.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON`, `404 NOT_FOUND`, `500 INTERNAL_ERROR`.

## Notes

Object access checks eligibility, existing reservation or participation even for users who also have a staff role.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
