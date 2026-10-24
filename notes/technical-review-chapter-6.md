# Chapter 6: CSRF — Technical and Security Review

Date: 2026-10-25  
Branch: improvements/chapters-engagement  
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/1

## Review scope

Source-based conceptual review of the Bengali theory-first chapter. The review covers the CSRF threat model, safe HTTP methods, ambient cookie authority, SameSite limitations, Fetch Metadata and Origin fallback policy, synchronizer tokens, login CSRF, logout, bearer Authorization header caveats, CORS, XSS/client-side CSRF boundaries, and the stated trade-offs.

## Decisions recorded

- State-changing actions use unsafe methods; GET only displays the order page. Logout is also a state-changing POST.
- KenaKata rejects state-changing requests whose Sec-Fetch-Site is cross-site, same-site, none, or an unknown value. If the header is absent, the configured target origin must exactly match Origin; missing headers or Origin: null fail closed. This is a KenaKata-specific threat-model decision, not a universal browser requirement.
- Fetch Metadata headers are browser-generated signals that JavaScript cannot set through the Fetch API, but a non-browser client can forge them. They are not authentication.
- A synchronizer token is random and independent of the session ID, bound to server-side session state, checked on state-changing requests, and never placed in URLs or logs.
- Login uses an anonymous pre-session token to reduce login CSRF; successful authentication invalidates the anonymous session and creates a new session ID and token.
- Framework CSRF protection may be a good starting point, but its defaults and missing-header behavior must be checked against the application's policy.

## Sources reviewed

- OWASP CSRF Prevention Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html
- MDN Cross-site request forgery: https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/CSRF
- MDN Fetch metadata: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Fetch_metadata
- W3C Fetch Metadata Request Headers: https://www.w3.org/TR/fetch-metadata/ (21 September 2026 Working Draft, not a Recommendation)
- WHATWG Fetch Standard: https://fetch.spec.whatwg.org/ (forbidden request-header names)
- RFC 7617 section 2.2: https://www.rfc-editor.org/rfc/rfc7617.html#section-2.2
- RFC 9110 section 9.2.1: https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.1
- Go vulnerability report GO-2025-3955: https://pkg.go.dev/vuln/GO-2025-3955 (affects CrossOriginProtection.AddInsecureBypassPattern in Go 1.25.0, fixed in Go 1.25.1)

## Limitations

No application code was compiled or executed. No automated tests, real-browser behavior, browser compatibility matrix, proxy behavior, multi-instance deployment, or production configuration was tested. A source-based conceptual review has been conducted and documented here. However, the chapter's Technical Review and Security Review checkboxes intentionally remain unchecked because the Chapter 6 repo notes explicitly direct that only Draft be checked at this stage. These checkboxes must not be interpreted as a claim that no review work occurred, nor as implementation audit or production security certification.

## Follow-up outside this chapter review

Before using the policy in a real application, verify browser-generated cookie and Fetch Metadata behavior in the supported browser matrix, check reverse-proxy handling of Origin/Fetch Metadata headers, run implementation-level tests, and assess rollout/false-positive behavior in a logging-only phase.
