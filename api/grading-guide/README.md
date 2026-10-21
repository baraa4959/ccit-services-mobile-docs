# University grading guide (reference only)

[← API catalog](../README.md) · [Conventions](../CONVENTIONS.md)

**`GET /api/mobile/v1/services/grading`** · **Auth:** bearer + active student. Implemented in accompanying v3 patch; deployment/integration verification still required.

## Request

No query or body.

## Success

200 `{"titleAr":"دليل التقديرات","sourceAr":"لائحة الدراسة والاختبارات بجامعة بيشة، الإصدار الثالث (1445هـ)، الملحق الأول، ص 50.","bands":[{"symbol":"A+","labelAr":"ممتاز مرتفع","min":95,"maxExclusive":101,"points5":5,"points4":4}],"special":[{"code":"IP","labelAr":"مستمر","countedInGpa":false,"passed":false}],"overall":[{"minimumGpa5":4.5,"labelAr":"ممتاز"}]}`. Arrays contain all official entries, shown examples abbreviated.

## Ownership, UI and business rules

Same static source as `/app/grading`; no individual transcript/course results, GPA calculation or grade-management endpoints.

## Errors

401 UNAUTHENTICATED; 403 STUDENT_ONLY. All error bodies are JSON `{"error":"CODE"}`; success payloads are direct objects or binary as noted.
