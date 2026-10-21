# CCIT Mobile API v1 — clickable endpoint catalog (v3 extension)

**Base:** `https://ccitstudents.com/api/mobile/v1`. Read [authentication](AUTHENTICATION.md), [conventions](CONVENTIONS.md) and [data models](DATA-MODELS.md) first. Rows link to full request, response, errors and ownership rules. New v3 paths below are available **only after deployment of the accompanying patch**.

## Authentication

| Method | Route | Contract |
|---|---|---|
| POST | `/auth/start` | [Start browser PKCE](auth-start/README.md) |
| POST | `/auth/approve` | [Web-browser approval only](auth-approve/README.md) |
| POST | `/auth/exchange` | [One-use code exchange](auth-exchange/README.md) |
| POST | `/auth/refresh` | [Refresh rotation](auth-refresh/README.md) |
| POST | `/auth/logout` | [Logout/revoke](auth-logout/README.md) |

## Home, profile and notifications

| Method | Route | Contract |
|---|---|---|
| GET | `/bootstrap` | [User, permissions, released services](bootstrap/README.md) |
| GET | `/me/profile` | [Own profile](profile/README.md) |
| GET | `/services/availability` | [Exactly four published student services](services-availability/README.md) |
| GET | `/me/notifications` | [Inbox and cursor](notification-list/README.md) |
| GET | `/me/notifications/unread-count` | [Unread badge](notification-count/README.md) |
| PATCH | `/me/notifications/{notificationId}/read` | [Mark own read](notification-read/README.md) |
| POST / DELETE | `/devices/push-token` | [Register/remove native token](device-token/README.md) |

## Activities

| Method | Route | Contract |
|---|---|---|
| GET | `/activities` | [Ordinary student list](activities/README.md) |
| GET | `/activities/{activityId}` | [Ordinary detail](activity-detail/README.md) |
| POST | `/activities/{activityId}/reserve` | [Reserve](activity-reserve/README.md) |
| POST | `/activities/{activityId}/withdraw` | [Withdraw](activity-withdraw/README.md) |
| GET | `/activities/{activityId}/attendance` | [Own attendance](activity-attendance/README.md) |
| POST | `/attendance/scan` | [QR + location](attendance-scan/README.md) |
| GET / POST | `/activities/{activityId}/review` | [Read/submit own attendance review with optional evidence](current-review/README.md) |
| GET | `/activities/{activityId}/review/{reviewId}/evidence` | [Download own evidence](current-review-evidence/README.md) |
| GET | `/activities/historical` | [Past CCIT list](historical-list/README.md) |
| GET | `/activities/historical/{id}` | [Historical detail](historical-detail/README.md) |
| GET / POST | `/activities/historical/{id}/reviews` | [Read/submit own attendance or points review](historical-review/README.md) |
| GET | `/activities/historical/{id}/reviews/{reviewId}/evidence` | [Download own historical evidence](historical-review-evidence/README.md) |

## Activity points

| Method | Route | Contract |
|---|---|---|
| GET | `/activities/points` | [Balance, earned, deducted](activity-points/README.md) |
| GET | `/activities/points/history` | [Recent point ledger](activity-history/README.md) |

## Student reports

| Method | Route | Contract |
|---|---|---|
| GET | `/services/reports` | [Self-service report options](reports-catalog/README.md) |
| GET | `/reports/statement?format=pdf\|xlsx` | [Own statement binary](report-statement/README.md) |
| GET | `/reports/certificates?ids=...` | [Own eligible certificates PDF](report-certificates/README.md) |

## Grading reference

| Method | Route | Contract |
|---|---|---|
| GET | `/services/grading` | [Official reference table](grading-guide/README.md) |

**Not supported:** academic plan, personal course grades, course catalogue, published timetables, course/visiting/training requests and any staff administrative mobile service. These are excluded from the released four-card product boundary, not hidden in this catalog by mistake.
