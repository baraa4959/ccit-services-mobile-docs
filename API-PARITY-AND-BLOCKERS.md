# Published-service parity and rollout checks

[← Home](README.md) · [API catalog](api/README.md)

`isProductionStudentBeta(userId)` on the **web** selects the four released student cards and blocks *unreleased* academic/administrative routes. The prior Mobile API erroneously applied that signal to every student service (`SERVICE_COMING_SOON`), including released activities and points. **The accompanying v3 backend patch corrects this:** mobile bearer student eligibility is verified by active STUDENT affiliation; only the four published services are exposed in `services.items`; no personal-grade/academic-plan/requests/schedules endpoints are published in mobile v3.

| Published web service | Mobile API contract after v3 patch |
|---|---|
| Activities | Ordinary list/detail/reserve/withdraw, QR, own attendance, own review + optional evidence, historical list/detail + review/evidence |
| Activity Points | Own totals/history |
| Reports | Student-only catalog; own statement PDF/XLSX, own eligible certificates PDF |
| Grading Guide | Read-only University grading reference; **no student-specific grade details** |
| Account shell | Browser PKCE auth; bootstrap/profile; own notifications and native APNs/FCM device registration |

The backend patch must still pass the actual project's `npm run test:all`, `npm run lint`, `npm run typecheck`, `npm run build`, migrations/deployment smoke test, and production-equivalent STUDENT-only/STAFF-only/dual-affiliation/disabled-account integration checks. This guide does not claim database/device/APNs/FCM verification. No staff administrative report/review route is exposed on mobile. Historical list includes past CCIT events that a student may need to review, not only recorded participations, matching the web.
