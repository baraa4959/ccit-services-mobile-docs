# Shared data models

[← Catalog](README.md)

| Model | Shape and semantics |
|---|---|
| Student service catalog | `items:[{id:"ACTIVITIES"|"ACTIVITY_POINTS"|"REPORTS"|"GRADING_GUIDE",labelAr,href,enabled:true}]` for active student; staff-only `items:[]` with `emptyMessage` |
| Ordinary activity | `id`, `status`, `definition` (title, venueName, startsAt, optional end, attendance configuration, points), `displayStatus`, `reservationOpen`, `reserved`, `participated`, `studentEligible`. Manager hints fixed false in list; detail strips editable/operable |
| Own attendance | `{registered,reviewState,missingSteps,enteredAt,exitedAt,complete,points}`; `missingSteps` ENTRY/EXIT as relevant |
| Own current review | `null | {id,reason,state,resolution,createdAt,hasEvidence}` |
| Historical list item | `{id,title,description,venue,heldAt,points,participants,participated,reviewPending,historical:true}` |
| Historical detail | `{activity:{id,title,description,venue,heldAt,points,participants,historical:true},participated,reviewState,canRequestReview,reviewCategory,certificateEligible}` |
| Own historical review | `null | {id,category,reason,state,resolution,createdAt,hasEvidence}` |
| Points | `{balance,earned,deducted}`; history `{items:[{id,activityId,title,delta,reason,createdAt,historical,administrative}]}` |
| Own reports | PDF/XLSX binary output, selected based on `format`; certificate list takes 1–20 distinct UUIDs |
| Grading guide | `titleAr`, `sourceAr`, `bands:[{symbol,labelAr,min,maxExclusive,points5,points4}]`, `special:[{code,labelAr,countedInGpa,passed}]`, `overall:[{minimumGpa5,labelAr}]` |

Review state is `PENDING|APPROVED|REJECTED|null`. Evidence is optional JPEG/PNG/PDF ≤2,000,000 bytes and never embedded in JSON; fetch by own-scoped evidence endpoint. Binary responses: check Content-Type, protect files from unintended backups/logging, never send user ID as an ownership selector.
