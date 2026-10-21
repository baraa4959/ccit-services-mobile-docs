# Submit student service request

**`POST /api/mobile/v1/services/requests`** · Bearer; active student and existing service grant

[← API catalog](../README.md)

## Request

JSON input validated using the existing CCIT `caseCreate` contract:
```json
{"kind":"TRAINING","placementCompany":"Example Organization","explanation":"A sufficiently detailed request"}
```
Optional UUID fields: `termId`, `courseId`, `sectionId`; optional strings: `externalInstitution`, `externalCourse`, `placementCompany`; mandatory `explanation` (10–4000 chars). For `ADD_COURSE`, `DROP_COURSE`, `CHANGE_SECTION`, provide `sectionId`; `VISITOR_OUT` requires `externalInstitution`; `TRAINING` requires `placementCompany`. Additional business checks (published registration window, duplicate open case, identity) remain server-side.

## Success response

`201 Created`: `{"id":"<uuid>","status":"SUBMITTED"}`.

## Errors

`400 INVALID_INPUT` / `INVALID`, `401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `FORBIDDEN` / `SERVICE_COMING_SOON`, `409 CONFLICT`, `500 INTERNAL_ERROR`.

## Notes

No requester/user ID parameter; the backend resolves the student identity. `VISITOR_IN` intentionally rejected.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
