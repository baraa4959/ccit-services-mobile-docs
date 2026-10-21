# Ordinary activity detail

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/02-ACTIVITIES.md)

**`GET /api/mobile/v1/activities/{activityId}`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

Path `{activityId}` valid UUID; no body.

**Body:** none.

## Success response

**HTTP 200** — example/wire shape:

```json
{"id":"<uuid>","collegeRef":"ccit","status":"PUBLISHED","definition":{"title":"Example","description":"...","venueName":"Venue","venueUrl":null,"startsAt":"<ISO8601>","endsAt":null,"publicationAt":null,"points":5,"capacity":null,"attendance":{"mode":"ENTRY_EXIT","qrMode":"ROTATING","qrSeconds":15,"walkInAllowed":false}},"displayStatus":"جارية","reservationOpen":false,"reserved":true}
```

## UI integration and business rules

**Illustrative partial** `definition` (actual object includes geofence and other attendance fields). Route checks list visibility first, filters `editable`/`operable` from service response. Render no manager controls. Activity `status` is backend enum; `displayStatus` is Arabic presentation label.

## Errors

400/404 invalid UUID yields 404 NOT_FOUND; 401 UNAUTHENTICATED, 403 STUDENT_ONLY, 404 NOT_FOUND, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
