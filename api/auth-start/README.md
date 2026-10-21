# Start sign-in

**`POST /api/mobile/v1/auth/start`** · Public (no Cookie)

[← API catalog](../README.md)

## Request

`Content-Type: application/json`. App generates and retains a cryptographically random PKCE verifier and sends its S256 challenge (base64url, 43 chars).
```json
{"codeChallenge":"<base64url-SHA256(verifier)>","codeChallengeMethod":"S256"}
```

## Success response

`201 Created`
```json
{"state":"<43-char random state>","authorizationUrl":"https://<web-origin>/mobile/connect?state=<state>","expiresIn":900}
```

## Errors

`400 INVALID_INPUT`, `429 RATE_LIMITED`, `503 UNAVAILABLE`.

## Notes

Open `authorizationUrl` using the OS system browser/authentication session. Save `state` with the verifier and compare returned state in constant time. The authorization request lasts 15 minutes; after approval the code lasts only two minutes. The web/public and app/callback origins are deployment-controlled, never caller inputs.

For authorization, secure storage, app links and status conventions, see [API authentication](../AUTHENTICATION.md).
