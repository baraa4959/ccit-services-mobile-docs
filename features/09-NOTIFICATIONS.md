# Notifications — inbox and native push

[Home](../README.md) · [Inbox](../api/notification-list/README.md) · [Unread](../api/notification-count/README.md) · [Read](../api/notification-read/README.md) · [Device](../api/device-token/README.md) · [Native push](../04-PUSH-NOTIFICATIONS.md)

## Screen interactions

| Element / action | Implementation |
|---|---|
| Unread badge | `GET /me/notifications/unread-count` → `{unreadCount:number}` |
| Inbox | `GET /me/notifications?limit=30`, then follow opaque `nextCursor` until null (max limit 50) |
| One row | `id`, `category`, `title`, `message`, nullable safe `href`, nullable `readAt`, `createdAt` |
| Open | `PATCH /me/notifications/{notificationId}/read` → `{ok:true}`, then navigate via recognized in-app destination; refresh count |
| No href | Open detail/text without creating speculative destination |
| Push | OS permission, APNs token on iOS / FCM token on Android, `POST /devices/push-token`, refresh on token rotation, `DELETE` on explicit deregistration |

Push delivery is a signal; authoritative data is the owner-scoped inbox. Never trust `notificationId` from push without fetching current user data. A user refusing push permission still has an in-app inbox. Handle app foreground/background/terminated tap routes and lock-screen presentation; never expose sensitive student details in additional client-generated alert text. Re-fetch on resume, push receipt and tap. The app's notification functionality belongs in the **current implementation**; backend owner will supply required Apple/FCM account configuration as it becomes available.
