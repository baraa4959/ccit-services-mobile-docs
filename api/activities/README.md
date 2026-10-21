# List ordinary student activities

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/02-ACTIVITIES.md)

**`GET /api/mobile/v1/activities`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

No query/body; current implementation does not support backend page/segment/search parameters.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"isStudent":true,"activities":[{"id":"<uuid>","collegeRef":"ccit","status":"PUBLISHED","definition":{"title":"Example activity","startsAt":"<ISO8601>","venueName":"Venue","points":5},"displayStatus":"قادمة","reservationOpen":true,"reserved":false,"participated":false,"studentEligible":true}]}
```

## UI integration and business rules

Item also carries the activity `definition`, dates and status metadata. Mobile list forces `manageable:false` and `operable:false`; never render staff controls. Filter local tabs: upcoming=reserved/participated & upcoming/running, previous=own completed/cancelled, discover=eligible upcoming/running not yet own. Underlying service cap 200; historical records are in the dedicated `/activities/historical` route.

## Errors

401 UNAUTHENTICATED, 403 STUDENT_ONLY, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
