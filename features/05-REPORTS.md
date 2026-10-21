# Student reports — التقارير

[← Home](../README.md) · [Report catalog](../api/reports-catalog/README.md) · [Own statement](../api/report-statement/README.md) · [Certificates](../api/report-certificates/README.md)

The student web reports page offers two report categories to ordinary students: **كشف رصيدي** (own activity points statement) and **شهادات الأنشطة** (one or several eligible activity certificates). Event-wide summaries, attendance lists, all-student balances, a different student's statement and management reports are **staff-only**, never exposed via Mobile API.

| Tool | Native behavior |
|---|---|
| Report list | `GET /services/reports` lists `POINTS_OWN` and `CERTIFICATES` and supported formats |
| Own statement | `GET /reports/statement?format=pdf` or `?format=xlsx`, authenticated bearer; binary download; no `userId` / target selector; missing institutional fields returns `MISSING_REPORT_FIELDS` |
| Certificates | Select 1–20 distinct eligible activity UUIDs; `GET /reports/certificates?ids=id1,id2&format=pdf`; server checks each ownership/attendance/positive points; ineligible → `CERTIFICATE_NOT_ELIGIBLE` 404 |

Open PDFs with a native viewer and allow save/share. Handle `Content-Type` and `Content-Disposition`, never try parsing a binary success as JSON; failures are JSON. Provide loading/progress and remove temp files. Reuse student activities/historical IDs, never send another user's ID.
