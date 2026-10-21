# 04 — Native Push Notifications (Required)

[← Home](README.md) · [Product behavior](02-PRODUCT-BEHAVIOR.md) · [Device registration API](api/device-token/README.md) · [Notification inbox API](api/notification-list/README.md)

**Implement native push in the current CCIT Services app for both iOS and Android.** The project owner is preparing the official Apple Developer membership and Firebase Cloud Messaging (FCM) project and will deliver the necessary **mobile-side configuration / signing access** as soon as they are ready. This is a configuration and live-test dependency, **not an instruction to omit or postpone coding the feature**. Do not build an alternative hosted backend or third-party notification database.

## Delivery architecture from the app's perspective

1. The signed-in app requests the OS notification permission at an appropriate point and obtains its platform **native provider token**.
2. It registers the token through [`POST /api/mobile/v1/devices/push-token`](api/device-token/README.md) using its current mobile Bearer token; re-register when the provider token changes or a new user session begins.
3. CCIT stores inbox messages authoritatively and its existing backend worker delivers visible alerts **directly through APNs (iOS) or FCM HTTP v1 (Android)**. The mobile application must **not** contain server APNs credentials, a Firebase service-account JSON, or any other sender-side private key.
4. The push carries `notificationId`. On receipt/tap, the app refreshes the authenticated inbox and navigates to a safe native destination based on the latest permitted notification record. Push does not grant access or create a second authentication system.
5. On sign-out, unregister the current session's provider token while authenticated, revoke the session via auth/logout, and clear local session data and notification-related local state.

## iOS (APNs)

- Implement notification authorization, denial/settings state, native remote-notification registration and APNs device-token capture. Use the **APNs token for this installed build**, formatted as the provider token string (normally hexadecimal), not an Expo Push Token or a token obtained from an unrelated sender.
- The owner will provide the authorized Apple Team / App ID and signing access. Use Bundle ID `com.ccitstudents.services`; configure Push Notifications capability and the appropriate provisioning/entitlements. The current free Personal Team can be used for initial UI/code work, but it is not acceptance proof for APNs or Associated Domains. Complete signed-device delivery tests when proper capabilities become available.
- Register `{ "platform": "IOS", "pushToken": "<apns-token>" }` through the CCIT API. Re-register on changed token or new session.
- Handle foreground notifications (choose an appropriate in-app presentation), background arrivals, user taps and cold-start notification responses. Do not depend on arbitrary silent/background execution to keep inbox state correct.
- The installed build's APNs environment (sandbox vs. production) must match the project owner's server-side configuration; report the build type during acceptance.

## Android (FCM)

- Integrate the owner-provided **FCM Android client configuration** for package `com.ccitstudents.services` (for example, Firebase client configuration consumed by the chosen Android framework). This is **not** the server service-account key.
- Implement notification permission handling, including Android 13+ `POST_NOTIFICATIONS` where applicable, and settings recovery after denial. Obtain and refresh the **FCM registration token**; do not submit an Expo Push Token.
- Register `{ "platform": "ANDROID", "pushToken": "<fcm-registration-token>" }`. Implement token rotation (`onNewToken` or framework equivalent) and appropriate registration after sign-in.
- Handle foreground display, background/terminated notification tap/open and data extraction; avoid duplicate visible notifications when the OS already displays the notification payload.
- Share the SHA-256 fingerprint of the certificate signing the *installed development build* with the owner for Android App Links. The owner will separately reconcile the production/Play Signing fingerprint.

## API contract and notification data

| Operation | Endpoint | Requirement |
|---|---|---|
| Register or update native device | [`POST /api/mobile/v1/devices/push-token`](api/device-token/README.md) | Bearer token and `{platform,pushToken}`; HTTP `200 {"ok":true}` |
| Remove current session's device registration | [`DELETE /api/mobile/v1/devices/push-token`](api/device-token/README.md) | Bearer token; HTTP `200 {"ok":true}` |
| Inbox | [`GET /api/mobile/v1/me/notifications`](api/notification-list/README.md) | Paginated source of truth; owns the message and read status |
| Unread count | [`GET /api/mobile/v1/me/notifications/unread-count`](api/notification-count/README.md) | Refresh unread badge on launch/resume/push |
| Read state | [`PATCH /api/mobile/v1/me/notifications/{notificationId}/read`](api/notification-read/README.md) | Mark read in the authenticated user scope |

The current server worker emits a generic preview (title **CCIT Services**, body **You have a new notification**) and `notificationId`. On iOS this is a custom top-level APNs field; on Android it is an FCM `data.notificationId` string. The display preview is **not** authoritative content. The actual title, message, category, `href`, creation time and read state come from the inbox API. Never navigate directly to an arbitrary `href`; map known relative destinations to supported native screens and otherwise show inbox details. Check access again after login/refresh if the app was opened by tapping a push. Duplicate or re-delivered events must not create duplicate inbox entries.

## UX and lifecycle

- Request notification permission with suitable context. Respect denial: no misleading success message or repeated prompts; explain how to enable permission in device settings. **The in-app inbox always remains usable** regardless of OS permission.
- On app foreground/resume, re-fetch unread count and current inbox page. Network failure leaves a clearly stale UI; never invent delivery or read confirmation.
- On sign-in/new session or provider token change, register that session's token. On sign-out, remove the registered token before revoking the bearer session where possible; also clear local state. Do not expose push tokens in logs, analytics or links.
- Locked screen: preserve the generic preview and avoid placing personal identity, points, or private service information in locally generated notification content.
- Avoid creating a second push service or third-party storage; CCIT backend is responsible for dispatch and storage.

## Required developer acceptance tests

| Scenario | Expected outcome |
|---|---|
| iOS/Android permission accepted or denied | OS setting respected; inbox works in either case |
| Login + token available | Correct native APNs/FCM token registered to current account/session |
| Provider token changes or user signs into a new session | New token registered; no stale account association |
| App foreground | Notification surfaced appropriately, inbox refreshed; no duplicate OS+local display |
| App background / device locked | Visible push arrives when system/network/provider permit |
| Terminated app, notification tapped | App opens, authenticates/refreshes as needed, and opens correct permitted inbox item |
| Re-delivery, missing or inaccessible notification ID | Idempotent refresh and safe fallback to inbox, no unauthorized detail disclosure |
| User signs out | Session's registration cleared; no stale user data exposed |
| iOS sandbox vs. production / Android signed build | Matching platform configuration verified against real installed apps |

Mocks and successful provider HTTP responses alone are **not** proof that the notification appeared on a locked device. Submit device test evidence when the owner supplies the provider configuration and signed builds.

## Information the project owner will provide

- Apple: official team/signing access and APNs-capable App ID for `com.ccitstudents.services` once available. Never ask for server `.p8` private key inside the mobile repository.
- Android: FCM project/client configuration appropriate to the mobile app (such as Android `google-services.json`, depending on the framework), **not** server service-account JSON.
- API/testing: authorized staging URL and test accounts, plus coordination for delivery smoke tests. Never add personal account data, production push tokens, provider sender secrets or service-account material to GitHub.
