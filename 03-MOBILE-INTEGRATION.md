# 03 — Mobile Integration & Signing (v1)

[← Home](README.md) · [Feature specifications](README.md#implemented-student-features) · [API parity](API-PARITY-AND-BLOCKERS.md) · [API catalog](api/README.md) · [Authentication](api/AUTHENTICATION.md)

## Fixed identifiers and URLs

| Setting | Value |
|---|---|
| Installed app label | **CCIT Services** |
| iOS Bundle Identifier | `com.ccitstudents.services` |
| Android `applicationId` / package | `com.ccitstudents.services` |
| API base | `https://ccitstudents.com/api/mobile/v1` (use separately supplied staging URL for non-production testing) |
| Web authentication | Use `authorizationUrl` returned by `POST /auth/start` |
| Approved return URL | `https://mobile.ccitstudents.com/auth/callback` |
| iOS associated domain | `applinks:mobile.ccitstudents.com` |
| Android verified link | HTTPS `mobile.ccitstudents.com`, path `/auth/callback` |

## Required implementation scope

Build UI, browser handoff, secure session/biometric unlock, profile, student services, activities, QR + location, **in-app notification inbox and complete native push integration** in this implementation. Include iOS APNs and Android FCM client configuration, OS notification permissions, push token lifecycle, authenticated device registration/removal, delivery handlers and tap navigation. The owner is arranging the official Apple Developer membership and FCM project and will provide the appropriate client configuration and signing access. Implement the client code without inventing or embedding provider server secrets; complete real-device delivery acceptance once these prerequisites are supplied. See [04 — Push notifications](04-PUSH-NOTIFICATIONS.md).

### Browser-to-app authentication

1. Generate a high-entropy PKCE verifier and S256 challenge; call `POST /api/mobile/v1/auth/start`.
2. Store verifier and the returned `state` for this attempt; launch the *returned* `authorizationUrl` using the OS-provided system browser/authentication session. Do not use an embedded WebView.
3. Existing website handles all credentials, account activation, Microsoft, Passkey and secondary verification. It renders the explicit approval page; the mobile app **does not call** the browser-only `/auth/approve` endpoint.
4. On verified HTTPS callback, validate `state`, extract one-time `code`, call `/auth/exchange` with verifier, store the token pair in OS secure storage, then call `/bootstrap`.
5. Serialize refresh calls; update rotated token pair atomically; on revoked/expired refresh return to website sign-in. Face ID/Touch ID only unlocks local credentials and does not establish server login.

See the exact [authentication contract](api/AUTHENTICATION.md).

### Signing: Apple free Personal Team (important)

The developer may start work immediately with a **free Apple Account** and Xcode **Personal Team**, and run eligible builds on a directly provisioned personal device. Apple documents that free provisioning profiles and app/device registrations expire after **7 days** and require periodic rebuilding. Free Personal Team does not provide TestFlight or App Store distribution. Apple describes advanced capabilities as depending on program membership; do **not** assume the free Personal Team can sign a build with working `com.apple.developer.associated-domains` entitlement or complete final Universal Link verification. In Xcode, inspect actual Signing & Capabilities, provisioning profile and signed entitlements; if unsupported or signing fails, mark iOS callback as **blocked pending organization Apple Developer Program membership** rather than claiming it has passed.

A free-team Team ID, even if displayed by Xcode, is **not proof** that Universal Links will work. Do not claim completion merely because the website returns `200` with an AASA document. Once the institution-owned paid team is available, set the same bundle ID under the authorized team, configure Associated Domains and retest installation and browser-to-app callback on a real device. Apple Team ID will change if signing moves to a different team. Do not create a new authentication mechanism or silently weaken HTTPS/state/PKCE checks to work around missing entitlements. Android sign-in can proceed independently while the iOS entitlement is pending.

Relevant Apple guidance: [free account / Personal Team](https://developer.apple.com/help/account/basics/about-your-developer-account/), [supported iOS capabilities](https://developer.apple.com/help/account/reference/supported-capabilities-ios/), [associated domains](https://developer.apple.com/documentation/xcode/supporting-associated-domains).

### Android signing and links

Android development signing is possible without a Google Play developer account. Supply the **SHA-256 fingerprint of the actual certificate signing the installed test build** to the project owner. The development and Play App Signing fingerprints may differ; provide production fingerprint separately when available. Configure verified App Links for `/auth/callback` on `mobile.ccitstudents.com` and test on an installed signed device. Never provide a keystore or private signing key to the documentation repo.

### Details to return to the project owner

- Apple: actual Xcode team name/Team ID, **free Personal Team or paid organization status**, confirmation whether Associated Domains entitlement can be enabled and is present in the signed test build, and any exact Xcode signing error. No Apple private keys or passwords.
- Android: actual signed-build application ID, SHA-256 certificate fingerprint, whether this is debug/development or release/Play App Signing, and the result of App Links verification. No keystore or private key.
- Confirm the signed app receives the exact callback URL on a real device (when signing permits); report Android and iOS outcomes separately.

## Acceptance checklist for developer team

- App label, identifiers, logo/colors and Arabic RTL match the design guide; correct empty/loading/error/accessibility states.
- Student-only navigation; staff-only has **no QR tab** and Services says **«لا توجد خدمات متاحة حاليًا»**; student+staff sees student features only.
- Browser-only PKCE with verified return link; reject mismatched `state`, duplicate code; secure storage; 15-minute access token, rotating refresh, 90-day idle expiration.
- API profile, services, activities, notification inbox/unread/mark-read function with authorized accounts.
- QR scanner requests camera and precise location; shows backend result and never treats `reviewRequired` as recorded attendance; no offline silent scan replay.
- Foreground/resume and sign-out correctly update server and local state. Test account deactivation/revocation and lost-network cases.
- Native push fully implemented: iOS APNs device token / Android FCM registration token; permission handling; device registration and token rotation; foreground/background/terminated presentation and tap navigation; inbox refresh/deduplication. Test on actual signed devices when the owner provides provider access and client configuration; do not claim delivery passed from mocks or provider acceptance alone.

Do not commit credentials, personal test accounts, API token samples, private signing material or service accounts.
