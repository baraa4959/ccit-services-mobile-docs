# Browser approval

**`POST /api/mobile/v1/auth/approve`** · **Web browser only**, Auth.js Cookie + same-origin Origin check

[← API catalog](../README.md)

## Request

`application/x-www-form-urlencoded` / form-data from `/mobile/connect` after login. Field `state=<43-character handoff state>`. Mobile app **must not** call this route.

## Success response

`303 See Other` to fixed deployment-owned HTTPS Universal/App Link `MOBILE_APP_CALLBACK_URL?state=...&code=...`. Code is one-use and short-lived; it is **not** an access token.

## Errors

`400 INVALID_INPUT`, `401 UNAUTHENTICATED`, `403 INVALID_ORIGIN`, `409 EXPIRED_OR_USED`, `500 INTERNAL_ERROR`.

## Notes

The browser renders an explicit approval screen, revalidates the authoritative web session and binds one-time code to the user. It never sends refresh/access tokens in a URL.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
