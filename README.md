# CCIT Services — Mobile Developer Documentation

Standalone implementation reference for the **CCIT Services** iOS and Android app. No access to the web frontend source repository is necessary. All app behavior and API contracts are specified here.

## Start here

| Document | What it covers |
|---|---|
| [01 — Visual identity](01-VISUAL-IDENTITY.md) | App name, supplied brand assets, palette, Arabic-first RTL typography and UI style |
| [02 — Product behavior](02-PRODUCT-BEHAVIOR.md) | Screens, role-based visibility, QR attendance, profile, services and in-app notifications |
| [03 — Mobile integration & signing](03-MOBILE-INTEGRATION.md) | App identifiers, browser handoff, Universal/App Links, signing and developer handoff |
| [04 — Push notifications](04-PUSH-NOTIFICATIONS.md) | Implement iOS APNs and Android FCM client support, permission/token lifecycle, secure registration, inbox navigation and acceptance tests |
| [API catalog](api/README.md) | Clickable, versioned endpoint list with a dedicated request/response page per operation |
| [API authentication](api/AUTHENTICATION.md) | PKCE, secure token storage, expiry/rotation, response/error conventions |

## Approved app configuration

| Setting | Value |
|---|---|
| Device display name | **CCIT Services** |
| iOS Bundle ID | `com.ccitstudents.services` |
| Android Application ID / package | `com.ccitstudents.services` |
| Current production API base | `https://ccitstudents.com/api/mobile/v1` |
| Existing browser login | `https://ccitstudents.com` (launch only the `authorizationUrl` returned by `auth/start`) |
| HTTPS return link | `https://mobile.ccitstudents.com/auth/callback` |
| V1 scope | Student self-service; staff may sign in and see profile, staff workflows unavailable |
| Push notifications | **Required in the current implementation.** Build the in-app inbox and full native push client integration for iOS (APNs) and Android (FCM). The project owner will provide provider project/client configuration and signed-app prerequisites as accounts become ready. |

**Development gate:** A free Apple Xcode Personal Team supports initial local coding and on-device tests but does **not** establish that production-like Universal Links / Associated Domains are usable in its provisioning profile. iOS end-to-end callback acceptance must be retested with appropriately provisioned Apple Developer Program signing once available. See [03 — Mobile integration](03-MOBILE-INTEGRATION.md).

## Working agreement

- The mobile app is a standalone client; do not embed the CCIT web login in a WebView and do not implement a separate native password/Microsoft/Passkey login screen.
- Mobile session tokens belong in platform-secure storage, not app settings, analytics, logs, URLs, or source control.
- Do not hardcode role permissions: refresh identity and enabled services from API. The server authorizes all mutations.
- Use the [API catalog](api/README.md) rather than scraping web pages or requesting the web frontend source.
- No backend deployment procedures, database migration instructions or infrastructure secrets are included in this repository. Provider-side credentials remain with the project owner; the mobile team receives only the appropriate platform client configuration and signing access.

**Contract caveat:** These documents describe the CCIT Mobile API v1 delivered for integration; local code checks are not a substitute for a signed-device, live API, network and release acceptance test. Staging credentials and any test accounts should be exchanged privately, never committed to this repository.
