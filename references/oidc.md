# OpenID Foundation

OpenID Connect specifications are updated by errata. The unversioned URLs below currently serve the errata-incorporated copies. Confirm that is still true before citing a paragraph, and prefer that copy over the original Final text when they differ.

The specification index is the place to see whether a newer errata set exists:

- [OpenID Connect specifications](https://openid.net/developers/specs/)

## OpenID Connect Core

- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)

Core is the identity layer on OAuth 2.0: the ID token and the claims about the end user. It is not a replacement for the OAuth security BCP.

## Discovery

- [OpenID Connect Discovery 1.0](https://openid.net/specs/openid-connect-discovery-1_0.html)

Discovery is how a relying party finds the provider’s endpoints and the location of its keys.

## JWKS-related specifications

`jwks_uri` is defined by OpenID Connect Discovery. The document at that URI is a JWK Set.

- [RFC 7517 — JSON Web Key (JWK)](https://www.rfc-editor.org/rfc/rfc7517)
- [OpenID Connect Discovery 1.0](https://openid.net/specs/openid-connect-discovery-1_0.html), for `jwks_uri`

Do not copy a sample key set into this repository.
