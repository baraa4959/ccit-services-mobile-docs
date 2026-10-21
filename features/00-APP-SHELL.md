# Application shell, sign-in and navigation

[Home](../README.md) · [Authentication contract](../api/AUTHENTICATION.md) · [Product scope](../02-PRODUCT-BEHAVIOR.md)

## User journeys

1. Fresh install → Welcome with **Sign in** only (no native username/password/Microsoft form).
2. Generate PKCE verifier/challenge → `POST /auth/start` → open returned `authorizationUrl` in OS-managed browser/auth session → existing web login and optional account activation/second factor → explicit web approval → associated HTTPS app link carrying `state` and one-use `code` → check matching pending `state` → `POST /auth/exchange` → persist both tokens securely → `GET /bootstrap`.
3. Returning user → local biometric/device credential prompt to unlock secure storage → use saved access token or serialized refresh → bootstrap on resume. Biometrics do **not** issue server authorization.
4. Sign out → unregister push device (if registered), `POST /auth/logout` with refresh token, erase locally stored credentials and cached personal data, show Welcome.

## Navigation contract

| Location | Student | Staff only | Data owner |
|---|---|---|---|
| Home | Visible | Identity/neutral empty home | `/bootstrap` |
| Activities | Visible if service available | Hidden | `/activities` |
| Attendance scanner | Student only | **Never** show | `/attendance/scan` |
| Services | Enabled catalog items | **«لا توجد خدمات متاحة حاليًا»** | `/services/availability` |
| Notifications | Visible | Visible | `/me/notifications` |
| Profile | Visible | Visible | `/me/profile` |

Do not hardcode API-derived service availability. Provide loading, empty, offline, forbidden and expired-session states on every network screen. Do not cache a staff grant to enable student features. Display profile affiliations independently (one user may be both). No staff management, administrative edits or manager QR controls in the mobile app.
