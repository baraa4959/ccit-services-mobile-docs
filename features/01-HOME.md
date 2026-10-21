# Home & services screen

[← Home](../README.md) · [Bootstrap](../api/bootstrap/README.md) · [Service catalog](../api/services-availability/README.md)

After browser sign-in, call `GET /bootstrap`; render `user.displayName`, the own notification badge, and `services.items`. For active student profiles, the service cards are only `ACTIVITIES`, `ACTIVITY_POINTS`, `REPORTS`, `GRADING_GUIDE`, with Arabic labels supplied by the API. For staff-only show the returned `emptyMessage` («لا توجد خدمات متاحة حاليًا»); don't guess staff privileges from role text.

The next activity comes from `GET /activities`: among `reserved || participated` with `displayStatus` «جارية»/«قادمة», prefer a running activity, otherwise ascending start date. Handle no activity, loading, expired session, no student activity profile and errors. QR is a student-only tab and never appears for staff only. Notifications/Profile remain accessible to every active signed-in user.
