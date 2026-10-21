# CCIT Services — Mobile Developer Handbook (v3)

**Audience:** mobile engineers, designers and QA. **Platform:** iOS and Android. **App name:** CCIT Services. **Identifier on both platforms:** `com.ccitstudents.services`.

This repository is the mobile team's standalone integration contract. All endpoints refer to the **accompanying v3 backend patch**, which must be applied and deployed before new v3 routes can be called. This documentation does not establish a successful live device/database or push delivery test. Mobile developers do not need the web frontend source.

## Implemented student features

The production web's **«خدماتك المتاحة»** panel contains exactly four published student services. The mobile Services screen MUST use `GET /api/mobile/v1/services/availability` (or bootstrap.services); do not show unreleased academic modules.

| Published service | UI and tools | API family |
|---|---|---|
| [Activities — الأنشطة](features/02-ACTIVITIES.md) | Next / previous / discover; details; reserve/withdraw; QR attendance; ordinary and historical reviews with optional evidence | [Activities](api/README.md#activities) |
| [Activity Points — رصيد الأنشطة](features/04-POINTS.md) | Balance, earned/deducted, history | [Points](api/README.md#activity-points) |
| [Reports — التقارير](features/05-REPORTS.md) | Own point statement PDF/XLSX; eligible activity certificates PDF | [Reports](api/README.md#student-reports) |
| [Grading Guide — دليل التقديرات](features/06-GRADING-GUIDE.md) | University grading bands, special grades, graduation ratings; **reference only, no individual course grades** | [Grading](api/README.md#grading-reference) |

**Shared screens:** [App shell & browser login](features/00-APP-SHELL.md), [Home](features/01-HOME.md), [QR & reviews](features/03-ATTENDANCE-AND-REVIEWS.md), [Notifications](features/09-NOTIFICATIONS.md), [Profile](features/10-PROFILE.md). Native push is **in the current implementation scope**, not deferred: [Push Notifications](04-PUSH-NOTIFICATIONS.md).

**Start with:** [Visual identity](01-VISUAL-IDENTITY.md) · [Product behavior](02-PRODUCT-BEHAVIOR.md) · [Mobile integration/signing](03-MOBILE-INTEGRATION.md) · [Clickable API catalog](api/README.md) · [API conventions](api/CONVENTIONS.md) · [Acceptance tests](ACCEPTANCE-TESTS.md) · [Backend/mobile parity](API-PARITY-AND-BLOCKERS.md).

Staff-only sign-in is permitted for profile and personal notifications, but no staff services or QR. Show «لا توجد خدمات متاحة حاليًا» for staff-only. Dual STUDENT+STAFF users see student features only when the server permits. Neither the client nor the token grants administrative operations through mobile routes.
