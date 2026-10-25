# Chapter 9: Technical and Security Review

Date: 2026-10-25
Branch: `improvements/chapters-engagement`
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/1

## Review scope

Source-based conceptual review of Chapter 9. Reviewed the product timeline, chapter boundaries, terminology, illustrative HTTP request, Mermaid diagrams, failure paths, and relevant IETF source text. This is not an implementation audit, browser/mobile test, or production security certification.

## Findings and changes

### 1. Password transport wording

The initial draft could imply that sending a password on every HTTPS request means the password is exposed in clear text on the network. Updated the paragraph to distinguish the design concern from transport confidentiality: HTTPS encrypts the transport, while repeatedly using a password still expands where it is sent and handled. The chapter now frames an access token as a common credential model, not as proof that tokens are automatically safer.

### 2. Token-validation and authorization failure paths

The first sequence diagram showed a combined success/permission-denied branch after token validation. Updated it to show missing or invalid token -> 401, then valid token -> authorization check, then allowed -> resource response or denied -> 403. The separate flowchart remains an educational simplification; exact API error behavior belongs to the API contract.

### 3. Intended resource / audience boundary

Added an explicit sentence that the server must check whether the token is intended for this API, as well as whether the user may access the requested resource and perform the action. This keeps token acceptance, token target, and resource authorization distinct without prematurely teaching JWT claim mechanics.

### 4. OAuth boundary

Chapter 9 explicitly says the KenaKata first-party mobile login illustration is not an OAuth authorization-code flow. RFC 6750 is used narrowly to explain bearer-token request semantics, not to imply that the whole KenaKata login flow is OAuth.

## Sources checked

- RFC 6750, especially Sections 1, 2.1, 3.1 and 5: bearer-token possession, the Authorization header, invalid-token / insufficient-scope responses, token disclosure, TLS and URL-token risks. https://www.rfc-editor.org/rfc/rfc6750.html
- RFC 6749: OAuth-context definition and framework context for access tokens. https://www.rfc-editor.org/rfc/rfc6749.html
- Project source map: `references/rfc.md`, including the explicit note that RFC 6750 is being used for bearer-token concepts and not as a claim that KenaKata's first-party login is OAuth.
- Project teaching policy: `docs/code-example-policy.md`, checked against the chapter's Mermaid diagrams, illustrative HTTP request, failure paths, and language-neutral approach.

## Review outcome

- No language-specific implementation code or runnable commands were added.
- The chapter preserves the product timeline: the existing server-rendered web app keeps its cookie session while the new mobile client uses the access-token model.
- The chapter avoids claiming that mobile clients cannot use cookies or that tokens are inherently safer than cookie sessions.
- The chapter keeps JWT internals, SPA token storage, expiry/replay details, refresh-token lifecycle, and OAuth/PKCE/OIDC flows in their planned later chapters.
- Mermaid source was inspected and the nested token-validation/authorization failure path was clarified. The diagram was not rendered in a Mermaid renderer.
- No application code, browser, or mobile-client tests were run. Review is conceptual and source-based only.

## Status

Chapter 9 is marked Draft, Technical Review (source-based), and Security Review (source-based threat-model review). Published remains unchecked. This record does not claim implementation testing or production security certification.