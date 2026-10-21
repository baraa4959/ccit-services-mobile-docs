# Own academic record summary

**`GET /api/mobile/v1/services/academic/my-plan`** · Bearer; active student

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{"students":[<own academic student summary>]}`. Records are filtered by existing `academic.students.own` permission and verified student-number ownership; expected fields include `id`, `academic_number`, `name`, `plan_id`, `program_name`, `department_name`, `level`, `cumulative_gpa`. May be an empty list if data is not linked.

## Errors

`401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `SERVICE_COMING_SOON` / `FORBIDDEN`, `500 INTERNAL_ERROR`.

## Notes

This v1 endpoint exposes the same own-summary data used by the existing service; not a promise of full course-by-course degree audit.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
