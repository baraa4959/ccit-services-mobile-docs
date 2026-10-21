# QR, attendance and evidence-backed reviews

[← Home](../README.md) · [Scan](../api/attendance-scan/README.md) · [Current review](../api/current-review/README.md) · [Historical review](../api/historical-review/README.md)

Student-only QR tab: request camera and precise foreground location only when scanning. Send signed QR string and measured latitude, longitude and horizontal accuracy in metres via `POST /attendance/scan`. User identity is derived from bearer session; do not send studentId, browser cookie or guest fields. `ok:true` means recorded phase; `ok:false, reviewRequired:true` means **NOT recorded/points not granted** and needs review. On uncertain network, fetch `GET /activities/{id}/attendance`, do not auto replay expired QR.

| Current activity | Endpoint / behavior |
|---|---|
| Read own state | `GET /activities/{id}/attendance`: registered, enteredAt, exitedAt, complete, reviewState, missingSteps, points |
| Read own request | `GET /activities/{id}/review` → `{review:null | {id,reason,state,resolution,createdAt,hasEvidence}}` |
| Submit own request | `POST /activities/{id}/review` with multipart `reason` and optional `evidence` (JPEG/PNG/PDF max 2 MB); HTTP 201 `{ok:true}` |
| Fetch own evidence | `GET /activities/{id}/review/{reviewId}/evidence` returns binary, authorized against own request |

| Historical activity | Endpoint / behavior |
|---|---|
| List and detail | `GET /activities/historical`, `GET /activities/historical/{id}` |
| Read own request | `GET /activities/historical/{id}/reviews` → `{review:null | {id,category,reason,state,resolution,createdAt,hasEvidence}}` |
| Submit own request | `POST /activities/historical/{id}/reviews` with multipart `category`, `reason`, optional `evidence`; server enforces category against actual participation |
| Fetch own evidence | `GET /activities/historical/{id}/reviews/{reviewId}/evidence` returns binary for owner |

Multipart constraints: request ≤2,500,000 bytes; attachment ≤2,000,000 bytes; allowed MIME and magic signature must match (PNG/JPEG/PDF); reason trim length 5–1500; only one review per person/activity. `PENDING`, `APPROVED`, `REJECTED` are authoritative states. The app never calls the web's same-origin cookie endpoint, never approves a review, and never counts PENDING as awarded points.

Handle `OUTSIDE_GEOFENCE`, `RESERVATION_REQUIRED`, `ACTIVITY_NOT_ELIGIBLE`, `STUDENT_SETUP_REQUIRED`, `INVALID_STATE`, `CONFLICT`, `INVALID_INPUT`, `INVALID_FILE`, `FILE_TOO_LARGE`, and `RATE_LIMITED` with distinct Arabic feedback. On 409 refresh the own review; do not blindly submit again.
