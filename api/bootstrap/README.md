# Initial profile and service navigation data

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/01-HOME.md)

**`GET /api/mobile/v1/bootstrap`** · **Auth:** bearer · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

No body, no query.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"user":{"id":"<uuid>","displayName":"...","accountStatus":"ACTIVE","identifiers":[],"affiliations":["STUDENT"],"roles":[],"permissions":[],"isStudent":true,"isStaff":false,"mobileStaffSupported":false},"services":{"items":[{"id":"ACTIVITIES","labelAr":"الأنشطة","enabled":true}],"emptyMessage":"لا توجد خدمات متاحة حاليًا","staffSupported":false,"releaseRestricted":false}}
```

## UI integration and business rules

Drive initial navigation from response, and refresh on app resume/role changes. `releaseRestricted` may be absent for staff-only. Neither cached flags nor roles authorize endpoint actions.

## Errors

401 UNAUTHENTICATED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).

**v3 service catalog clarification:** the nested `services.items` returns `ACTIVITIES`, `ACTIVITY_POINTS`, `REPORTS`, `GRADING_GUIDE` for active students, and an empty array for staff-only. No unreleased academic workflows are advertised.
