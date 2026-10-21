# Product behavior — released mobile scope

[← Home](README.md) · [Full feature descriptions](README.md#implemented-student-features)

Student home and Services must reflect the exact four published web cards: Activities (`/app/activities`), Activity Points (`/app/points`), Reports (`/app/reports`), and Grading Guide (`/app/grading`). Do **not** implement My Academic Plan, personal grades, catalog, schedules, course requests, visiting students, training or administration as supported services. Their web cards are future/unreleased for an ordinary student.

| Screen | Student tools | Staff only |
|---|---|---|
| Sign-in | System browser → web login → PKCE link handoff → local biometric unlock | Same sign-in |
| Home | Greeting, notification badge, four service cards, next registered activity | Identity and own notifications; empty Services |
| Activities | My upcoming, previous (ordinary + historical), discover; reservation; detail; own status; reviews | No QR or activity management |
| QR tab | Camera scan + device geolocation; authoritative server response | Hidden |
| Points | Totals, recent ledger, filters | Hidden |
| Reports | Own statement and eligible certificates only | No student reports |
| Grading | Static university reference returned by endpoint, not student course grades | Services empty |
| Notifications | Inbox, mark read, native APNs/FCM | Own inbox |
| Profile | Identity, affiliations, roles/scopes, sign out | Own profile |

A browser-login session and the mobile session are separate. Use the documented bearer API and never call cookie-based web endpoints from the native app. A normal STUDENT in production should be able to use the four published services; the production beta gate restricts **unreleased** routes, not these four. Server permission checks remain authoritative for enrollment, review ownership, reports and attendance.
