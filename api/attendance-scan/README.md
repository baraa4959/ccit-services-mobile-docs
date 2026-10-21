# Scan attendance QR

**`POST /api/mobile/v1/attendance/scan`** · Bearer; active student; rate-limited per mobile session

[← API catalog](../README.md)

## Request

`application/json`:
```json
{"qrToken":"<signed QR payload>","latitude":20.0,"longitude":42.0,"accuracyMeters":12.5}
```
The coordinates shown are syntactic examples only, **not** an approved college geofence. Obtain consent for precise location immediately before scan. The server identifies the user from bearer credentials; no `guestName`, `guestNumber`, `claimId` or user ID accepted.

## Success response

`200 OK` on recorded attendance: `{"ok":true,"verified":true,"pointsEligible":true,"phase":"ENTRY"}` (phase may be `EXIT`). If indoor GPS is insufficient but the existing service opens an exception: `{"ok":false,"reviewRequired":true,"phase":"ENTRY","verified":true,"pointsEligible":false}`. **Review is not completed attendance.**

## Errors

`400 INVALID_INPUT`, `401 UNAUTHENTICATED`, `403 STUDENT_ONLY` / `OUTSIDE_GEOFENCE` / `ACTIVITY_NOT_ELIGIBLE` / `RESERVATION_REQUIRED` / `STUDENT_SETUP_REQUIRED` / `SERVICE_COMING_SOON`, `409 INVALID_STATE` (e.g. invalid/expired QR or wrong phase), `409 CONFLICT` (e.g. duplicate attendance or capacity), `429 RATE_LIMITED`, `500 INTERNAL_ERROR`.

## Notes

Reuses the authoritative signed QR claim and single-use completion service in one server request. Existing 10-minute claim, timing windows, geofence/accuracy rules, student eligibility, reservation and capacity all remain enforced. No cookie, anonymous or guest mobile flow. If a network response is lost, refresh current activity state rather than claiming success or blindly replaying a scan.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
