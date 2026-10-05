# IETF / RFC

Canonical text lives at the RFC Editor. Datatracker HTML is convenient, but the RFC Editor copy is the one to cite. Check errata before treating a sentence as current.

Where an older RFC and a Best Current Practice disagree, follow the BCP and say that the older document is the base protocol.

## OAuth 2.0

- [RFC 6749 — The OAuth 2.0 Authorization Framework](https://www.rfc-editor.org/rfc/rfc6749)
- [RFC 6750 — The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://www.rfc-editor.org/rfc/rfc6750)

RFC 6749 is the base protocol. It is not the current security guidance by itself.

## PKCE

- [RFC 7636 — Proof Key for Code Exchange](https://www.rfc-editor.org/rfc/rfc7636)

## JWT

- [RFC 7519 — JSON Web Token (JWT)](https://www.rfc-editor.org/rfc/rfc7519)
- [RFC 7515 — JSON Web Signature (JWS)](https://www.rfc-editor.org/rfc/rfc7515)
- [RFC 7517 — JSON Web Key (JWK)](https://www.rfc-editor.org/rfc/rfc7517)

## JWT Best Current Practices

- [RFC 8725 — JSON Web Token Best Current Practices](https://www.rfc-editor.org/rfc/rfc8725)

## OAuth Security Best Current Practice

- [RFC 9700 — Best Current Practice for OAuth 2.0 Security](https://www.rfc-editor.org/rfc/rfc9700)

RFC 9700 updates and extends the threat model and security advice in RFC 6749, RFC 6750, and RFC 6819. It deprecates some modes of operation. Read it before repeating advice taken from older OAuth material.

- [RFC 6819 — OAuth 2.0 Threat Model and Security Considerations](https://www.rfc-editor.org/rfc/rfc6819) is the earlier threat catalog. Use it together with RFC 9700, not instead of it.

## DPoP

- [RFC 9449 — OAuth 2.0 Demonstrating Proof of Possession (DPoP)](https://www.rfc-editor.org/rfc/rfc9449)

## Related token documents

These are not a second protocol family. They are the usual places to check lifetime, revocation, and what an access token is allowed to look like.

- [RFC 7009 — OAuth 2.0 Token Revocation](https://www.rfc-editor.org/rfc/rfc7009)
- [RFC 7662 — OAuth 2.0 Token Introspection](https://www.rfc-editor.org/rfc/rfc7662)
- [RFC 9068 — JSON Web Token (JWT) Profile for OAuth 2.0 Access Tokens](https://www.rfc-editor.org/rfc/rfc9068)
