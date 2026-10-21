# Profile, roles and session

[Home](../README.md) · [Profile API](../api/profile/README.md) · [Bootstrap](../api/bootstrap/README.md) · [Logout](../api/auth-logout/README.md)

Show `displayName`, `accountStatus`, and `identifiers[]` (types `INSTITUTIONAL_EMAIL`, `STUDENT_NUMBER`, `EMPLOYEE_NUMBER`; `primary` flag) for current bearer owner only. Show all **active** `affiliations[]` (`STUDENT`, `STAFF`), `roles[]` (`code`, `nameAr`, `scopes[]`) and optionally informational `permissions[]` (`roleCode`, `code`, `scope`). A `null` scope must **not** mean global authorization. Staff-only users can view their profile despite staff operation UIs being unsupported.

Profile actions: refresh user details, local biometric lock settings (device-only), sign out (unregister device token when possible, `POST /auth/logout`, delete locally cached credentials and personal content). No native password change, TOTP setup, Passkey registration or account activation UI: these remain web flows. Do not expose controls from mere presence of `roleCode`; every functional endpoint enforces its own permission. Dual-affiliation accounts can use student routes only if the server's active student + relevant grants allow them.
