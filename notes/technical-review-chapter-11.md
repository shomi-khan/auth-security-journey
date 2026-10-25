# Chapter 11: Technical ও Security Review

Date: 2026-10-25  
Branch: improvements/chapters-engagement  
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/1

## Review scope

Chapter 11-এর source-based conceptual Technical Review ও Security Review। Chapter 10-এর SPA/browser token-storage narrative, KenaKata-র product timeline, chapter naming/link conventions, token expiry semantics, bearer-token replay, revocation, JWT validation এবং RFC references যাচাই করা হয়েছে।

এটি application implementation audit, automated test run, Mermaid renderer test বা production security certification নয়।

## Continuity ও repository conventions

1. **Chapter path:** Root README এবং Season 1 index-এর canonical path হলো 11-token-expiration-replay/README.md; article ভুল করে 11-token-expiration-replay-window/README.md-এ ছিল। Article canonical path-এ সরানো হয়েছে, যাতে বিদ্যমান সূচির links কাজ করে।
2. **Chapter 10 continuity:** Chapter 10 browser storage এবং XSS/CSRF boundary নিয়ে; Chapter 11 token-এর lifetime, replay এবং revocation নিয়ে। Chapter 11 OAuth refresh-token flow-কে current KenaKata implementation বলে না এবং refresh-token lifecycle পরের নির্ধারিত chapter-এ রাখে।
3. **Product continuity:** Mobile/API access-token story বজায় আছে। OAuth-specific RFC-গুলো conceptual reference হিসেবে চিহ্নিত, KenaKata-র first-party flow-কে OAuth implementation বলা হয়নি।
4. **Learning format:** KenaKata scenario, Mermaid sequence diagram, Decision Game, reasoning, edge case, self-check এবং concise key takeaway রাখা হয়েছে।
5. **Status:** Draft ও source-based Technical/Security Review complete; Published unchecked। এটি published chapter বা production audit নয়।

## Technical Review

1. **Bearer-token replay:** RFC 6750-এর bearer model অনুযায়ী token-এর possession নিজেই ব্যবহার করার ক্ষমতা দিতে পারে; token leak হলে token এখনও accepted থাকা সময় replay risk থাকে। Endpoint-এর authorization আলাদা করে প্রয়োজন।
2. **JWT expiration:** RFC 7519 §4.1.4 অনুযায়ী exp উপস্থিত থাকলে current time expiration time-এর আগে হতে হবে। exp claim JWT format-এ optional; application/profile যদি এটি require করে, missing exp reject করার policy স্পষ্ট থাকা দরকার।
3. **Clock skew:** RFC 7519 সীমিত leeway অনুমোদন করে, সাধারণত কয়েক মিনিটের বেশি নয়। Chapter-এ tolerance-কে bounded configuration হিসেবে ব্যাখ্যা করা হয়েছে, lifetime বাড়ানোর shortcut হিসেবে নয়।
4. **JWT validation:** Signature verification একা issuer trust বা intended audience প্রমাণ করে না। Trusted key/issuer relationship, acceptable algorithm, audience এবং application/profile-এ required claims independently validate করতে হয়।
5. **iat/nbf distinction:** iat issuance time জানায়; একে সর্বক্ষেত্রে mandatory expiry claim হিসেবে ধরা হয়নি। nbf ও iat validation application/profile policy অনুযায়ী প্রযোজ্য হবে।
6. **Expiration vs revocation:** Local JWT validation-এ server-side revocation state না দেখলে token expiry পর্যন্ত accepted থাকতে পারে। Denylist, introspection বা account-level invalidation-এর effectiveness architecture এবং state propagation-এর ওপর নির্ভর করে।
7. **DPoP limits:** RFC 9449 sender-constraining ও proof replay detection-এর mechanism বর্ণনা করে, কিন্তু DPoP-কে blanket replay immunity বলা হয়নি। DPoP proof-এর short acceptance window এবং jti replay tracking-এর সীমাও উল্লেখ করা হয়েছে।
8. **Authorization boundary:** Valid token, valid signature, বা sub claim request presenter-কে নিজে থেকে মানুষের পরিচয়ের প্রমাণ দেয় না; resource/action authorization ও incident evidence আলাদা concern।

## Sources checked

- RFC 7519, JSON Web Token, especially §§4.1.4–4.1.7 (exp, nbf, iat, jti): https://www.rfc-editor.org/rfc/rfc7519.html
- RFC 8725, JSON Web Token Best Current Practices, especially §§3.1, 3.8 and 3.9 (algorithm verification, issuer and audience validation): https://www.rfc-editor.org/rfc/rfc8725.html
- RFC 6750, The OAuth 2.0 Authorization Framework: Bearer Token Usage, bearer-token possession and token protection: https://www.rfc-editor.org/rfc/rfc6750.html
- RFC 9449, OAuth 2.0 Demonstrating Proof of Possession (DPoP), sender-constraining and DPoP proof replay: https://www.rfc-editor.org/rfc/rfc9449.html
- RFC 7009, OAuth 2.0 Token Revocation, OAuth-specific revocation endpoint and token revocation semantics: https://www.rfc-editor.org/rfc/rfc7009.html
- OWASP OAuth 2.0 Cheat Sheet, sender-constrained token and refresh-token rotation guidance: https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html

## Outcome and limitations

- Technical Review: complete, source-based conceptual review.
- Security Review: complete, source-based threat-model review.
- Canonical chapter path and navigation consistency: corrected.
- Mermaid diagrams were reviewed as source text only; no Mermaid renderer was run.
- No application, mobile, browser or automated test suite was run.
- This record is not a production security audit or certification.
