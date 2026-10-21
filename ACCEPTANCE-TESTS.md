# Mobile acceptance matrix — published services only

[← Home](README.md) · [API](api/README.md)

| Case | Expected outcome |
|---|---|
| Ordinary active student with production flags | All four cards visible; can read authorized activities and points; no false `SERVICE_COMING_SOON` |
| STAFF only | Can sign in/profile/own inbox; four student services not shown, QR hidden; «لا توجد خدمات متاحة حاليًا» |
| Dual STUDENT+STAFF | Student tools only, no staff writes/queues or reports |
| Sign-in | OS-managed browser, one-use code + matching PKCE state, bearer exchange, biometric local unlock, 90-day idle policy |
| Ordinary activities | Upcoming/previous/discover, search/filter, reserve/withdraw, read own attendance and points |
| Current review | Missing attendance steps; request while activity is running when eligible, attach valid JPEG/PNG/PDF ≤2 MB, one request, own state/evidence, notification after manager decision |
| Historical | List and detail; not participated → ATTENDANCE review; participated → POINTS review; unique request and own evidence; cannot approve own request |
| QR | Server verifies token, phase, location, precision, student entitlement; error or review-needed is not success |
| Points | Totals match independent ledger despite last-200 history cap |
| Reports | Own statement PDF/XLSX; own eligible certificates PDF; cannot specify target user or generate manager reports |
| Grading | Official band/special/overall reference only; no transcript/course grade page |
| Notifications & push | Own inbox/read/unread, native provider token registration, foreground/background/locked device tests once provider accounts are configured |
| Disabled/logout | Revoked session no longer authorizes any route; push registration removed appropriately |

**Verification boundary:** local tests and static source checks are not a substitute for Staging PostgreSQL/Redis, signed-device universal/app links, PDF runtime, and APNs/FCM end-to-end checks.
