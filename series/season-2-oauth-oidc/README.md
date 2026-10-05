# Season 2 — OAuth, OIDC & Delegated Identity

Chapters 16–21. None of these chapters are written yet.

## Purpose

Move from “KenaKata checks its own password” to delegated access and federated sign-in. OAuth 2.0 is how an application gets limited access. OpenID Connect is how a provider tells KenaKata who signed in. Google login is that design applied to a real identity provider, not a separate kind of login.

The product background is in [`../../product/README.md`](../../product/README.md). Google login and connected applications ship only at the stages in [`../../product/timeline.md`](../../product/timeline.md).

## Chapters

16. [OAuth 2.0](16-oauth/README.md) — OAuth 2.0 as delegated access.
17. [Authorization Code ও state](17-authorization-code-state/README.md) — The authorization code flow and `state`.
18. [Redirect URI ও Code Theft](18-redirect-uri-code-theft/README.md) — Redirect URI checking and authorization code theft.
19. [PKCE](19-pkce/README.md) — PKCE for a public client.
20. [OpenID Connect](20-oidc/README.md) — OpenID Connect as an identity layer.
21. [Google Login](21-google-login/README.md) — Google login for KenaKata.

## After this season

The reader should be able to:

- Explain OAuth 2.0 as delegated authorization, and say what it does not establish by itself
- Explain why `state`, redirect URI checking, and PKCE exist
- Explain what an ID token adds, and how that differs from an access token
- Place Google as an external identity provider, and a connected application as an OAuth client of KenaKata

[Series index](../../README.md)
