# Activity Points — رصيد الأنشطة

[← Home](../README.md) · [Totals](../api/activity-points/README.md) · [History](../api/activity-history/README.md)

`GET /activities/points` returns `{balance,earned,deducted}` from ordinary activity ledger, historical credits/adjustments and administrative corrections. `GET /activities/points/history` returns `{items:[{id,activityId,title,delta,reason,createdAt,historical,administrative}]}`; limited to 200 newest rows without a server cursor. Display earned when delta > 0, deducted when delta < 0; provide All/Earned/Deducted filters locally and order by createdAt. Do not compute total from the capped history. Historical entries navigate to `/activities/historical/{activityId}` in the mobile app; admin-only adjustments have no activity-detail navigation. Point redemption is not an implemented feature.
