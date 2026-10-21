# Read own profile and effective grants

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/10-PROFILE.md)

**`GET /api/mobile/v1/me/profile`** · **Auth:** bearer · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

No body, no userId.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"id":"<uuid>","displayName":"Example","accountStatus":"ACTIVE","identifiers":[{"type":"STUDENT_NUMBER","value":"000000","primary":true}],"affiliations":["STUDENT"],"roles":[{"code":"...","nameAr":"...","scopes":[{"type":"OWN","reference":null}]}],"permissions":[{"roleCode":"...","code":"...","scope":{"type":"OWN","reference":null}}],"isStudent":true,"isStaff":false,"mobileStaffSupported":false}
```

## UI integration and business rules

Identifiers: `STUDENT_NUMBER`, `EMPLOYEE_NUMBER`, `INSTITUTIONAL_EMAIL`; roles can have multiple scopes. `scope:null` is not global authorization. Staff roles display as information only.

## Errors

401 UNAUTHENTICATED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
