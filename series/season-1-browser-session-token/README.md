# Season 1 — The Browser, the Session & the Token

Chapters 1–15. None of these chapters are written yet.

## Purpose

Follow KenaKata from a simple shop to a browser sign-in, then through the failures of that cookie session, and into access tokens and refresh tokens. Organizations and authorization close the season, after the reader has something worth authorizing.

The product background is in [`../../product/README.md`](../../product/README.md). What may already exist in a given chapter is in [`../../product/timeline.md`](../../product/timeline.md).

## Chapters

1. [কেন Authentication দরকার?](01-why-authentication/README.md) — Why authentication is necessary, and the initial KenaKata application.
2. [Authentication বনাম Authorization](02-authentication-vs-authorization/README.md) — Authentication and authorization as different questions.
3. [Password-এর নিরাপত্তা](03-password-security/README.md) — Storing and checking passwords.
4. [Session ও Cookie](04-session-and-cookie/README.md) — The server session and the browser cookie.
5. [Session Hijacking ও Session Fixation](05-session-hijacking-fixation/README.md) — Hijacking and fixation of that session.
6. [CSRF](06-csrf/README.md) — Cross-site request forgery.
7. [XSS ও HttpOnly](07-xss-and-httponly/README.md) — Cross-site scripting and the HttpOnly flag.
8. [Token](08-tokens/README.md) — Tokens, and why the mobile client needs them.
9. [JWT](09-jwt/README.md) — JWT as a token format.
10. [SPA-তে Token রাখা](10-spa-token-storage/README.md) — Where a single-page app keeps tokens.
11. [Token Expiration ও Replay](11-token-expiration-replay/README.md) — Expiration and replay.
12. [Refresh Token](12-refresh-token/README.md) — Refresh tokens.
13. [Refresh Token Rotation](13-refresh-token-rotation/README.md) — Rotation and reuse detection.
14. [Logout ও Revocation](14-logout-revocation/README.md) — Logout and revocation.
15. [IDOR, BOLA ও Authorization](15-idor-bola-authorization/README.md) — IDOR, BOLA, and authorization.

## After this season

The reader should be able to:

- Say what authentication establishes, and what authorization still has to decide
- Describe a cookie-backed session and the distinct failures of theft, fixation, CSRF, and XSS
- Describe an access token and a refresh token, including storage, expiry, rotation, and revocation
- Explain why a signed-in user, or a role string, is not yet permission to touch a particular resource

[Series index](../../README.md)
