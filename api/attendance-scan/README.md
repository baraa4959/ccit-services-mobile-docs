# Complete native QR and GPS check

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md) · [Related feature](../../features/03-ATTENDANCE-AND-REVIEWS.md)

**`POST /api/mobile/v1/attendance/scan`** · **Auth:** bearer + student · **Status:** implemented in accompanying v3 patch; integration tests still required.

## Request and ownership

JSON strict `{qrToken:string (30–1600),latitude:number [-90,90],longitude:number [-180,180],accuracyMeters:number [0,100000]}`; max body 4096 chars.

**Example input** (placeholder credentials/IDs must be replaced):

```json
{"qrToken":"<signed-QR-payload>","latitude":20.0,"longitude":42.0,"accuracyMeters":12.5}
```

## Success response

**HTTP 200** — example/wire shape:

```json
{"ok":true,"verified":true,"pointsEligible":true,"phase":"ENTRY"}
```

## UI integration and business rules

Another accepted result: `{ "ok":false,"reviewRequired":true,"phase":"ENTRY","verified":true,"pointsEligible":false }` for insufficient-location-accuracy review. **Neither values above nor coordinates are a real venue/geofence.** Backend does QR signature/window/phase, claim, eligibility, reservation, geofence and one-time completion. Never submit an offline cached scan; reconcile with attendance GET when response lost.

## Errors

400 INVALID_INPUT, 401 UNAUTHENTICATED, 403 STUDENT_ONLY/OUTSIDE_GEOFENCE/ACTIVITY_NOT_ELIGIBLE/RESERVATION_REQUIRED/STUDENT_SETUP_REQUIRED, 409 INVALID_STATE/CONFLICT, 429 RATE_LIMITED, 500 INTERNAL_ERROR. Return body is `{"error":"CODE"}`. Handle status and code together; the listed codes describe relevant route/downstream paths and are not permission bypass hints.

**See also:** [Authentication and token security](../AUTHENTICATION.md). For website functionality without an equivalent bearer route, review the [parity table](../../API-PARITY-AND-BLOCKERS.md).
