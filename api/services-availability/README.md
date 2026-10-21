# Four published student services

[← API catalog](../README.md)

**`GET /api/mobile/v1/services/availability`** · Auth: bearer. No body/query.

**Student HTTP 200:** `{"items":[{"id":"ACTIVITIES","labelAr":"الأنشطة","href":"/app/activities","enabled":true},{"id":"ACTIVITY_POINTS","labelAr":"رصيد الأنشطة","href":"/app/points","enabled":true},{"id":"REPORTS","labelAr":"التقارير","href":"/app/reports","enabled":true},{"id":"GRADING_GUIDE","labelAr":"دليل التقديرات","href":"/app/grading","enabled":true}],"emptyMessage":"لا توجد خدمات متاحة حاليًا","staffSupported":false}`.

**Staff-only HTTP 200:** `{"items":[],"emptyMessage":"لا توجد خدمات متاحة حاليًا","staffSupported":false}`. Home bootstrap embeds the same `services` payload. This is the published web Student Beta catalog; no academic plan, personal grades, catalogue, schedules or requests are part of it. Error: 401 for invalid bearer, 500 for infrastructure failure.
