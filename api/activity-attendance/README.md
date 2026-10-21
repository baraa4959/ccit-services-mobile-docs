# Own attendance state for activity

**`GET /api/mobile/v1/activities/{activityId}/attendance`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body; `{activityId}` is a UUID.

## Success response

`200 OK`: current attendance status including `registered`, `reviewState`, `missingSteps`, `enteredAt`, `exitedAt`, `complete`, `points`.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON`, `404 NOT_FOUND` / `403 FORBIDDEN`, `500 INTERNAL_ERROR`.

## Notes

Use after a network interruption during QR scanning to reconcile actual server state instead of treating an absent HTTP response as success.

See [API authentication](../AUTHENTICATION.md).
