# Paginated notification inbox

**`GET /api/mobile/v1/me/notifications`** · Bearer access token

[← API catalog](../README.md)

## Query / path parameters

`limit`: optional integer 1–50 (default 30); `cursor`: optional opaque token returned by previous page. Unknown query keys are rejected.

## Request

No body.

## Success response

`200 OK`
```json
{"items":[{"id":"<uuid>","category":"ACTIVITY","title":"...","message":"...","href":"/app/activities/<uuid>","readAt":null,"createdAt":"<ISO-8601>"}],"nextCursor":"<opaque-cursor-or-null>"}
```
`href` is an existing safe relative web destination, not an instruction to open an arbitrary URL. Map known destinations to native screens; otherwise show inbox details.

## Errors

`400 INVALID_INPUT` or `INVALID_CURSOR`, `401 UNAUTHENTICATED`, `500 INTERNAL_ERROR`.

## Notes

Owner-scoped keyset pagination sorted newest-first. Follow the returned opaque cursor; it is not an offset nor an identity token. Null `nextCursor` means end.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
