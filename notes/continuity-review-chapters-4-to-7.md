# Season 1: Chapters 4–7 Continuity Review

**Date:** 2026-10-25  
**Branch:** `improvements/chapters-engagement`  
**Scope:** Story timeline, product architecture, security-control boundaries, chapter transitions, terminology, and relative links across Chapters 4–7.  
**Review type:** Source-based editorial and conceptual review. This is not a browser test, implementation audit, or production security certification.

## Summary

Chapters 4–7 tell a coherent progression:

1. **Chapter 4:** The server needs a cookie-backed session so the user does not have to authenticate with a password on every request.
2. **Chapter 5:** A valid session ID can be stolen or fixed; expiry, revocation, and rotation reduce risk but do not solve every request-origin problem.
3. **Chapter 6:** CSRF can abuse the victim's browser without the attacker knowing the session ID.
4. **Chapter 7:** XSS is different: attacker-controlled script runs in the trusted page context, so HttpOnly and CSRF controls alone do not solve it.

The review found and fixed two continuity issues, made the Chapter 7-to-8 transition explicit, and aligned Chapter 4's cookie-RFC citation with Chapter 5's current-standard reference.

## Fixes applied

### 1. Chapter 5's timeline

**Issue:** Chapter 5 said the session logging work had been done “গত সপ্তাহে” after Chapter 4. The story moves from Monday in Chapter 4 to Tuesday in Chapter 5, so “last week” contradicted the timeline.

**Fix:** Changed it to “গতকাল”.

File: `series/season-1-browser-session-token/05-session-hijacking-fixation/README.md`

### 2. Cookie-standard references

**Issue:** Chapter 4 described RFC 6265 as the published cookie RFC, while Chapter 5 already cited RFC 10025 as the current standard and RFC 6265 as historical.

**Fix:** Chapter 4's reference list now identifies RFC 10025 as the current-standard reference and labels RFC 6265 historical. The note explicitly says current-standard section details should be checked against RFC 10025 rather than assuming every historical RFC 6265 detail is unchanged.

File: `series/season-1-browser-session-token/04-session-and-cookie/README.md`

### 3. Chapter 7's broken transition wording

**Issue:** Chapter 7 said “F অধ্যায়ে” instead of identifying the previous chapter.

**Fix:** Changed the phrase to “অধ্যায় ৬-এ” and added an explicit link to Chapter 8, including a short bridge that avoids presenting tokens as automatically safer than cookie sessions.

File: `series/season-1-browser-session-token/07-xss-and-httponly/README.md`

## Cross-chapter security consistency

| Concept | Continuity outcome |
|---|---|
| Session ID and cookie | Chapter 4 establishes the credential; Chapter 5 treats it as a bearer-like secret that must be rotated/revoked appropriately. |
| Session hijacking vs fixation | Chapter 5 distinguishes post-login theft from pre-login attacker-known ID. |
| CSRF | Chapter 6 correctly frames it as browser request abuse without requiring the attacker to know the session ID. |
| Safe HTTP methods | Chapter 6 demonstrates why state-changing GET routes create a CSRF path even when SameSite=Lax prevents a cross-site POST cookie. |
| HttpOnly | Chapters 4 and 7 agree: it hides the cookie value from JavaScript APIs, but does not prevent script in the page from causing browser requests. |
| XSS vs CSRF | Chapter 6 says XSS can undermine its CSRF controls; Chapter 7 explains why same-origin attacker script can access page-available data and attempt authenticated actions. |
| Authorization | Chapters 5–7 preserve the distinction between authenticated identity and permission to perform a particular operation. |
| Product timeline | Chapters 4–7 remain in the single server-rendered web-app / cookie-session phase; mobile, SPA and token architecture are not treated as already deployed. |

## Link verification

Checked the relative “next chapter” links in Chapters 4–7 against the working branch:

- Chapter 4 → Chapter 5: exists
- Chapter 5 → Chapter 6: exists
- Chapter 6 → Chapter 7: exists
- Chapter 7 → Chapter 8: exists

This checks that the linked files exist, not that GitHub successfully renders every anchor or Mermaid diagram.

## Remaining limitations / follow-up

- No browser or Mermaid-renderer run was performed.
- No application source code or deployed cookie/CSP configuration was audited.
- The RFC 10025 citation alignment does not by itself establish that every detailed cookie claim in Chapter 4 has been revalidated against the new RFC. Before publication, perform a focused claim-to-section pass against the current RFC 10025 text and update any section references that differ.
- This review does not certify the series for production use.

## Result

The narrative timeline and chapter transitions are consistent after the edits. The key security boundaries across session management, CSRF, and XSS are aligned at the conceptual level. Chapters 4–7 remain drafts/reviewed educational material, not published or implementation-certified guidance.
