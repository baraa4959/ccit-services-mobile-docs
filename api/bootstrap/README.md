# Bootstrap app identity and features

**`GET /api/mobile/v1/bootstrap`** · Bearer access token

[← API catalog](../README.md)

## Request

No body.

## Success response

`200 OK`: `{ "user": <Profile>, "services": <ServicesAvailability> }`; profile includes `id`, `displayName`, `accountStatus`, `identifiers`, `affiliations`, `roles`, `permissions`, `isStudent`, `isStaff`, `mobileStaffSupported:false`. Services described in the [availability contract](../services-availability/README.md).

## Errors

`401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`.

## Notes

Call after exchange and after role/account changes or app resume; never treat cached permissions as server authority. A person can have BOTH STUDENT and STAFF affiliation.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
