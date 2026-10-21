# CCIT Mobile API v1 — Endpoint Catalog

**Base path:** `/api/mobile/v1` on the existing CCIT backend. The sections below link directly to a self-contained Markdown README for **every endpoint**. The native team requires only these docs and the supplied branding asset; do not consume the web-source repository.

Start with [API authentication, session lifecycle and error contract](AUTHENTICATION.md), then [product behavior](../02-PRODUCT-BEHAVIOR.md) and [mobile integration/signing](../03-MOBILE-INTEGRATION.md).

## Authentication

| Method | Endpoint | Detailed contract |
|---|---|---|
| `POST` | [`/api/mobile/v1/auth/start`](auth-start/README.md) | [Start sign-in](auth-start/README.md) |
| `POST` | [`/api/mobile/v1/auth/approve`](auth-approve/README.md) | [Browser approval](auth-approve/README.md) |
| `POST` | [`/api/mobile/v1/auth/exchange`](auth-exchange/README.md) | [Exchange sign-in code](auth-exchange/README.md) |
| `POST` | [`/api/mobile/v1/auth/refresh`](auth-refresh/README.md) | [Rotate session credentials](auth-refresh/README.md) |
| `POST` | [`/api/mobile/v1/auth/logout`](auth-logout/README.md) | [Sign out current device](auth-logout/README.md) |

## User / profile

| Method | Endpoint | Detailed contract |
|---|---|---|
| `GET` | [`/api/mobile/v1/bootstrap`](bootstrap/README.md) | [Bootstrap app identity and features](bootstrap/README.md) |
| `GET` | [`/api/mobile/v1/me/profile`](profile/README.md) | [Read own profile](profile/README.md) |

## Notifications

| Method | Endpoint | Detailed contract |
|---|---|---|
| `GET` | [`/api/mobile/v1/me/notifications`](notification-list/README.md) | [Paginated notification inbox](notification-list/README.md) |
| `GET` | [`/api/mobile/v1/me/notifications/unread-count`](notification-count/README.md) | [Unread count](notification-count/README.md) |
| `PATCH` | [`/api/mobile/v1/me/notifications/{notificationId}/read`](notification-read/README.md) | [Mark notification as read](notification-read/README.md) |

## Student services

| Method | Endpoint | Detailed contract |
|---|---|---|
| `GET` | [`/api/mobile/v1/services/availability`](services-availability/README.md) | [Available student services](services-availability/README.md) |
| `GET` | [`/api/mobile/v1/services/requests`](services-requests/README.md) | [Own student service requests](services-requests/README.md) |
| `POST` | [`/api/mobile/v1/services/requests`](services-request-create/README.md) | [Submit student service request](services-request-create/README.md) |
| `GET` | [`/api/mobile/v1/services/schedules`](services-schedules/README.md) | [Published schedules](services-schedules/README.md) |
| `GET` | [`/api/mobile/v1/services/academic/my-plan`](services-my-plan/README.md) | [Own academic record summary](services-my-plan/README.md) |
| `GET` | [`/api/mobile/v1/services/catalog`](services-catalog/README.md) | [Published academic catalog](services-catalog/README.md) |

## Activities and QR

| Method | Endpoint | Detailed contract |
|---|---|---|
| `GET` | [`/api/mobile/v1/activities`](activities/README.md) | [List student activities](activities/README.md) |
| `GET` | [`/api/mobile/v1/activities/{activityId}`](activity-detail/README.md) | [Read activity detail](activity-detail/README.md) |
| `POST` | [`/api/mobile/v1/activities/{activityId}/reserve`](activity-reserve/README.md) | [Reserve activity](activity-reserve/README.md) |
| `POST` | [`/api/mobile/v1/activities/{activityId}/withdraw`](activity-withdraw/README.md) | [Withdraw activity reservation](activity-withdraw/README.md) |
| `GET` | [`/api/mobile/v1/activities/points`](activity-points/README.md) | [Own points balance](activity-points/README.md) |
| `GET` | [`/api/mobile/v1/activities/points/history`](activity-history/README.md) | [Own points history](activity-history/README.md) |
| `GET` | [`/api/mobile/v1/activities/{activityId}/attendance`](activity-attendance/README.md) | [Own attendance state](activity-attendance/README.md) |
| `POST` | [`/api/mobile/v1/attendance/scan`](attendance-scan/README.md) | [Scan attendance QR](attendance-scan/README.md) |

## Device / push — required

| Method | Endpoint | Detailed contract |
|---|---|---|
| `POST / DELETE` | [`/api/mobile/v1/devices/push-token`](device-token/README.md) | [Register or remove push device](device-token/README.md) |

## Access conventions

- **Public:** start and exchange (exchange requires one-time code + PKCE), refresh/logout use rotating refresh credential. Browser approval requires a current Auth.js cookie and same-origin POST.
- **Bearer:** profile, inbox, availability, activities, services and device registration use `Authorization: Bearer <accessToken>`. Only student-affiliated accounts can call student service/attendance routes; staff-only mobile services are intentionally unavailable.
- **Response security:** JSON `no-store`; stable error codes, owner-scoped SQL, business checks in server services. Refresh tokens are never passed in URL or sent to read endpoints.
- **Release scope:** implement the notification inbox and native push-token registration/delivery in the current app. The project owner supplies provider setup and client configuration when ready; verify on signed devices and confirm deployed API environment before acceptance. See [mobile push guide](../04-PUSH-NOTIFICATIONS.md).

See [API authentication](AUTHENTICATION.md) for a complete handoff example, storage and verification details.
