# 02 — Expected Mobile Product Behavior

**Scope:** Student self-service only in v1. Staff login can succeed and staff identity/roles appear in profile, but staff workflows are **not supported** in the app. A staff-only account sees no QR scanner and an empty Services screen: **«لا توجد خدمات متاحة حاليًا»**. A dual-affiliation account may use *student* features when its STUDENT affiliation remains active; no staff operations are displayed. Never use UI hiding as an authorization control; the backend enforces student-only routes.

## Navigation

| Screen | Expected behavior |
|---|---|
| Welcome | One **Sign in** button opens the existing CCIT web login in the **system browser/auth session**, not an embedded WebView. Web handles existing Microsoft/passkey/password + 2FA/activation flows. |
| Unlock | After initial sign-in, use local Face ID/Touch ID/device credentials to unlock securely stored mobile credentials. The server, not biometrics, determines if a session is valid. After 90 days of inactivity, require web sign-in again. |
| Home | User greeting, relevant student service shortcuts, unread badge; no invented metrics. |
| Activities | Eligible published activities, activity details, reservation/withdrawal and points balance. Reuse existing eligibility/business rules. |
| Attendance | **Student-only tab.** Scan a QR using camera, request precise location, submit QR + coordinates and accuracy using the signed-in account, display backend result. No guest attendance mode in mobile v1. |
| Services | Show only enabled student services returned by availability. Catalog, own academic plan, published schedules and student request categories supported by the API. Respect server feature flags/release restrictions. Staff-only: «لا توجد خدمات متاحة حاليًا». |
| Notifications | Paginated inbox, unread count, mark read, native push permission/token registration, notification presentation and tap navigation. Fetch authoritative details from the API using notification ID. Do not blindly open arbitrary web URLs. |
| Profile | Identity, verified identifiers, active student/staff affiliation(s), roles and scopes; show permissions as informative data, not local enforcement. Sign out revokes device session and clears device push registration. |

## Attendance feedback

Successful attendance: show whether entry/exit was recorded; `reviewRequired: true` means a location accuracy review was created, **not successful attendance/points**. Handle `OUTSIDE_GEOFENCE`, `RESERVATION_REQUIRED`, `ACTIVITY_NOT_ELIGIBLE`, `STUDENT_SETUP_REQUIRED`, `CONFLICT` (e.g. duplicate/full), `INVALID_STATE` (e.g. bad/expired QR or closed activity), and `RATE_LIMITED` individually. GPS permission denied/inaccurate requires actionable feedback. All geofence/capacity/eligibility verification is server-side.

## Notifications — required functionality

Implement the complete notification experience in the current app: inbox pagination, unread badge, mark-as-read, OS notification permission prompt, native push token acquisition and registration, token rotation, foreground/background/terminated-state handling, and opening the relevant in-app destination when a notification is tapped. iOS uses its native **APNs device token**; Android uses its **FCM registration token**. Do not register an Expo push token. Re-fetch authoritative inbox content on app opening/resume and when a push is received or tapped.

Push is a delivery signal, not the data source: parse the `notificationId` sent by the provider, fetch current owner-scoped notifications via the API, and deduplicate by notification ID. Use the generic server notification preview on the lock screen; do not expose extra personal details in locally constructed push alerts. A declined OS notification permission must not disable the in-app inbox. Implement the complete client flow now; the owner will supply the Apple/Firebase prerequisites for end-to-end tests as they become available. See [04 — Push notifications](04-PUSH-NOTIFICATIONS.md).

## Error/offline rules

Refresh an expired 15-minute access token once using the current rotating refresh token; serialize concurrent refresh calls. A `401` after refresh requires web login. Never silently submit a QR scan offline, guess a successful attendance, or grant staff rights based on an old profile response. Display backend error codes as localized user-facing messages.
