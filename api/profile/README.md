# Read own profile

**`GET /api/mobile/v1/me/profile`** · Bearer access token

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `<Profile>`
```json
{"id":"<uuid>","displayName":"...","accountStatus":"ACTIVE","identifiers":[{"type":"STUDENT_NUMBER","value":"...","primary":true}],"affiliations":["STUDENT","STAFF"],"roles":[{"code":"...","nameAr":"...","scopes":[{"type":"OWN","reference":null}]}],"isStudent":true,"isStaff":true,"mobileStaffSupported":false,"permissions":[{"roleCode":"...","code":"...","scope":{"type":"OWN","reference":null}}]}
```
Other identifier types: `EMPLOYEE_NUMBER`, `INSTITUTIONAL_EMAIL`. A role/permission may carry multiple scopes. `scope` can be `null` for assignments with no scope row; this is *not* an implicit global grant.

## Errors

`401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`.

## Notes

Only data belonging to the authenticated user is returned. Staff operations remain unsupported in mobile v1 regardless of displayed permission codes.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
