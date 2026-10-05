# Auth Security Journey

A story-driven Bengali technical series on authentication, authorization, OAuth, OIDC, and production security.

## What this repository is

This is a 25-chapter Bengali technical series. It follows one fictional product, **KenaKata**, from a simple online shop to a multi-tenant SaaS platform.

The chapters are the series. The files under [`product/`](product/README.md) keep that story consistent. Nothing in the chapter index is written yet. A checkbox stays open until that chapter is actually published.

## Learning philosophy

Each chapter starts from a problem inside KenaKata:

```text
Problem
  ↓
Naive Solution
  ↓
Failure
  ↓
Threat
  ↓
Better Solution
  ↓
Trade-off
```

A control is introduced because the previous design failed, and the chapter stops at the trade-off. It does not treat the new control as a final answer.

## Seasons

### Season 1 — The Browser, the Session & the Token

Chapters 1–15. A browser sign-in, a cookie session, the attacks on that session, and the move to access tokens and refresh tokens.

[Season 1](series/season-1-browser-session-token/README.md)

### Season 2 — OAuth, OIDC & Delegated Identity

Chapters 16–21. Delegated access, OpenID Connect, and Google login.

[Season 2](series/season-2-oauth-oidc/README.md)

### Season 3 — Production Security

Chapters 22–25. MFA, machine-to-machine authentication, and the production architecture.

[Season 3](series/season-3-production-security/README.md)

## Product

KenaKata’s overview, characters, domain model, timeline, and final architecture:

[product/](product/README.md)

## Chapter index

`[ ]` means the chapter has not been written.

### Season 1 — The Browser, the Session & the Token

- [ ] [01. কেন Authentication দরকার?](series/season-1-browser-session-token/01-why-authentication/README.md)
- [ ] [02. Authentication বনাম Authorization](series/season-1-browser-session-token/02-authentication-vs-authorization/README.md)
- [ ] [03. Password-এর নিরাপত্তা](series/season-1-browser-session-token/03-password-security/README.md)
- [ ] [04. Session ও Cookie](series/season-1-browser-session-token/04-session-and-cookie/README.md)
- [ ] [05. Session Hijacking ও Session Fixation](series/season-1-browser-session-token/05-session-hijacking-fixation/README.md)
- [ ] [06. CSRF](series/season-1-browser-session-token/06-csrf/README.md)
- [ ] [07. XSS ও HttpOnly](series/season-1-browser-session-token/07-xss-and-httponly/README.md)
- [ ] [08. Token](series/season-1-browser-session-token/08-tokens/README.md)
- [ ] [09. JWT](series/season-1-browser-session-token/09-jwt/README.md)
- [ ] [10. SPA-তে Token রাখা](series/season-1-browser-session-token/10-spa-token-storage/README.md)
- [ ] [11. Token Expiration ও Replay](series/season-1-browser-session-token/11-token-expiration-replay/README.md)
- [ ] [12. Refresh Token](series/season-1-browser-session-token/12-refresh-token/README.md)
- [ ] [13. Refresh Token Rotation](series/season-1-browser-session-token/13-refresh-token-rotation/README.md)
- [ ] [14. Logout ও Revocation](series/season-1-browser-session-token/14-logout-revocation/README.md)
- [ ] [15. IDOR, BOLA ও Authorization](series/season-1-browser-session-token/15-idor-bola-authorization/README.md)

### Season 2 — OAuth, OIDC & Delegated Identity

- [ ] [16. OAuth 2.0](series/season-2-oauth-oidc/16-oauth/README.md)
- [ ] [17. Authorization Code ও state](series/season-2-oauth-oidc/17-authorization-code-state/README.md)
- [ ] [18. Redirect URI ও Code Theft](series/season-2-oauth-oidc/18-redirect-uri-code-theft/README.md)
- [ ] [19. PKCE](series/season-2-oauth-oidc/19-pkce/README.md)
- [ ] [20. OpenID Connect](series/season-2-oauth-oidc/20-oidc/README.md)
- [ ] [21. Google Login](series/season-2-oauth-oidc/21-google-login/README.md)

### Season 3 — Production Security

- [ ] [22. MFA ও Step-up Authentication](series/season-3-production-security/22-mfa-step-up/README.md)
- [ ] [23. Machine-to-Machine Authentication](series/season-3-production-security/23-machine-to-machine/README.md)
- [ ] [24. Security Context ও Architecture](series/season-3-production-security/24-context-and-architecture/README.md)
- [ ] [25. Capstone](series/season-3-production-security/25-capstone/README.md)

## Writing rules

### Language

Primary language: Bengali.

Technical terms can remain in English when that is clearer.

### Style

- narrative-driven
- problem-first
- technically rigorous
- beginner-accessible
- production-aware
- occasional subtle humor
- no meme-heavy writing

### Security

Never use unexplained absolute claims such as “JWT is secure.”

Always explain:

- threat model
- security property
- limitation
- trade-off

### Major concepts

Every major security technology should explain:

- what it is
- why it exists
- what problem it solves
- what it does not solve
- where it lives
- how it can be attacked
- damage if compromised
- lifetime
- revocation
- alternatives
- production trade-offs

The working rules for adding a chapter are in [CONTRIBUTING.md](CONTRIBUTING.md).

## Repository map

| Path | Purpose |
| --- | --- |
| [series/](series/) | Season and chapter manuscripts. [Season 1](series/season-1-browser-session-token/README.md), [Season 2](series/season-2-oauth-oidc/README.md), [Season 3](series/season-3-production-security/README.md) |
| [`product/`](product/README.md) | KenaKata continuity |
| [`references/`](references/README.md) | Authoritative sources to check before a claim |
| [`diagrams/`](diagrams/authentication/) | Reusable diagrams, added only when a chapter needs one |
| [`examples/go/`](examples/go/authentication/) | Small Go examples, added only when a chapter needs one |
| [`notes/`](notes/glossary.md) | Glossary, misconceptions, and editorial decisions |

## License

[LICENSE](LICENSE)
