# Activities — full student workflow

[← Home](../README.md) · [API catalog](../api/README.md#activities) · [Attendance & reviews](03-ATTENDANCE-AND-REVIEWS.md)

The web's **StudentActivitiesHome** divides activities into:

| Tool | Exact behavior / API |
|---|---|
| Next activity | From `GET /activities`, registered/participated, «جارية» preferred then nearest «قادمة» |
| «أنشطتي القادمة» | Own `reserved || participated`, statuses «قادمة»/«جارية», ascending by `definition.startsAt` |
| «الأنشطة السابقة» | Ordinary own completed/cancelled from `GET /activities` **plus historical records from** `GET /activities/historical` (even unrecorded past events may be discoverable for review) |
| «استكشف الأنشطة» | Student-eligible upcoming/running, not reserved/participated; filter within returned set |
| Search/filter | Title/venue and display status; ordinary listing is bounded to 200; historical listing also max 200; no server cursor currently |
| Ordinary detail | `GET /activities/{activityId}`, includes definition, publication/display status and reservation state, never present manager controls |
| Reserve / withdraw | `POST /activities/{id}/reserve` and `/withdraw`; on success refresh list/detail; a reservation alone is not attendance |
| Own attendance and review | `GET /activities/{id}/attendance`, `GET/POST /activities/{id}/review`, optionally evidence download |
| Historical detail | `GET /activities/historical/{id}`, shows title, description, venue, heldAt, points, participant count, participation, review state, `certificateEligible` |
| Historical review | `GET/POST /activities/historical/{id}/reviews` with category ATTENDANCE (not yet participated) or POINTS (participated), optional evidence, one request per person/activity; decisions are staff-only on web |
| Certificate | If eligible, `GET /reports/certificates?ids={uuid}` PDF, never infer eligibility from a display badge alone |

**Current ordinary activity review:** minimum 5 / maximum 1500 characters reason, optional JPEG/PNG/PDF ≤2,000,000 bytes. Server checks actual reservation/attendance, missing attendance step, event eligibility, and enforces one request per activity. Review can be filed while an activity is running when a reviewable step is missing. It does not immediately grant attendance/points.

**Historical review:** only active CCIT student profile. ATTENDANCE when not participated, POINTS when participated; unique request per historical activity. Approved ATTENDANCE adds historical participant; approved POINTS does **not** silently award points and may require manager adjustment. Re-fetch state and points after decision/notification. Details/list and own review return only student data; staff-only review queues/approval tools are NOT in the mobile API.
