# Available student services

**`GET /api/mobile/v1/services/availability`** · Bearer access token

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`, student: `{"items":[{"id":"ACTIVITIES","labelAr":"الأنشطة","enabled":true}, ...],"emptyMessage":"لا توجد خدمات متاحة حاليًا","staffSupported":false,"releaseRestricted":false}`. Other possible `id`: `ACADEMIC_PLAN`, `SCHEDULES`, `ACADEMIC_CATALOG`, `STUDENT_REQUESTS`. Staff-only user: `{"items":[],"emptyMessage":"لا توجد خدمات متاحة حاليًا","staffSupported":false}`.

## Errors

`401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`.

## Notes

Only enabled services should be shown. Existing production student release restrictions can produce empty items with `releaseRestricted:true`; never bypass this in the app. Dual-affiliation users may use student features only.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
