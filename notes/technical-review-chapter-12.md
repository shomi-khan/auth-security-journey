# Chapter 12: Technical ও Security Review

Date: 2026-10-25  
Branch: improvements/chapters-engagement  
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/1

## Review scope

Chapter 12-এর source-based conceptual Technical Review ও Security Review। Chapter 11-এর SPA/browser token-storage narrative, KenaKata-র product timeline, chapter naming/link conventions, token expiry semantics, bearer-token replay, revocation, JWT validation এবং RFC references যাচাই করা হয়েছে।

এটি application implementation audit, automated test run, Mermaid renderer test বা production security certification নয়।

## Continuity ও repository conventions

1. **Chapter path:** Root README এবং Season 1 index-এর canonical path হলো 11-token-expiration-replay/README.md; article ভুল করে 11-token-expiration-replay-window/README.md-এ ছিল। Article canonical path-এ সরানো হয়েছে, যাতে বিদ্যমান সূচির links কাজ করে।
2. **Chapter 11 continuity:** Chapter 11 browser storage এবং XSS/CSRF boundary নিয়ে; Chapter 12 token-এর lifetime, replay এবং revocation নিয়ে। Chapter 12 OAuth refresh-token flow-কে current KenaKata implementation বলে না এবং refresh-token lifecycle পরের নির্ধারিত chapter-এ রাখে।
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


## Follow-up review: Chapter 12 in PR #2

Date: 2026-10-25  
Branch: `docs/chapters-11-25-blueprints`  
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/2

Chapter 12-এর existing draft, Chapter 12 brief, Chapter 11 continuity, Chapter 13 boundary এবং product timeline আবার মিলিয়ে দেখা হয়েছে। এই follow-up-এ একটি narrative ambiguity ঠিক করা হয়েছে: password বদলানোর পর পুরোনো access token দিয়ে request সফল হওয়ার ঘটনাটি দেখে যেন পাঠক ধরে না নেয় যে password change সব architecture-এ access token স্বয়ংক্রিয়ভাবে invalidate করে। এখন opening-এ স্পষ্ট বলা আছে, এটি architecture ও revocation policy-র ওপর নির্ভর করে; password change নিজে থেকে সব already-issued access token বাতিল করে না।

এই পরিবর্তনটি আলাদা commit-এ রাখা হয়েছে। মূল article-এর বাকি scope, learning sequence ও security claims-এ অপ্রয়োজনীয় rewrite করা হয়নি। এটি source-based conceptual follow-up; implementation test, link checker, Mermaid renderer বা production audit চালানো হয়নি।


## Final Technical Review ও Security Review: Chapter 12 in PR #2

Date: 2026-10-25  
Branch: `docs/chapters-11-25-blueprints`  
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/2

### Review coverage

এই pass-এ Chapter 12-এর সম্পূর্ণ article ও chapter brief, Chapter 11-এর SPA token-storage context, Chapter 13-এর refresh-token scope এবং `product/timeline.md` মিলিয়ে দেখা হয়েছে। মূল security claims-গুলো authoritative RFC guidance-এর সঙ্গে পুনরায় cross-check করা হয়েছে।

### Technical findings

1. **JWT expiration semantics:** `exp` claim উপস্থিত থাকলে exp time-এর সমান বা পরে token গ্রহণ না করার বর্ণনা RFC 7519-এর সঙ্গে সামঞ্জস্যপূর্ণ। `exp` optional claim, bounded clock-skew allowance এবং application/profile-এ missing-exp policy নির্ধারণের কথাও সঠিকভাবে আলাদা করা হয়েছে.
2. **JWT validation:** Signature verification-কে একা যথেষ্ট বলা হয়নি। Trusted key, allowed algorithm, issuer, audience এবং application/profile-এর প্রয়োজনীয় claims যাচাইয়ের কথা আছে। JWT decode করা আর validate করার পার্থক্যও স্পষ্ট.
3. **Bearer replay:** Token চুরি হলে তা এখনও গ্রহণযোগ্য থাকা অবস্থায় পুনরায় উপস্থাপনের ঝুঁকি বোঝানো হয়েছে; resource/action authorization-কে token validity থেকে আলাদা রাখা হয়েছে.
4. **Expiry, session এবং revocation:** এগুলোকে পৃথক concern হিসেবে উপস্থাপন করা হয়েছে। Stateless local validation-এ revocation state না দেখলে token expiry পর্যন্ত accepted থাকতে পারে, এই caveat-টি যথাযথ.
5. **Clock skew ও time claims:** Clock-skew tolerance সীমিত রাখার কথা বলা হয়েছে; `iat`-কে universal expiry mechanism হিসেবে দেখানো হয়নি.
6. **Sender-constrained tokens:** DPoP ও mTLS-কে OAuth-context example হিসেবে সীমাবদ্ধ রাখা হয়েছে। DPoP-কে blanket replay immunity বলা হয়নি; proof validity window এবং `jti` replay tracking-এর ভূমিকা উল্লেখ আছে.
7. **Illustrative payload:** JWT payload-টি বাস্তব credential নয়, উদাহরণ হিসেবে চিহ্নিত; timestamp-গুলো 15-minute example-এর সঙ্গে সামঞ্জস্যপূর্ণ.

### Security findings

1. **Token leakage:** Log, compromised device/endpoint এবং অন্য exposure path উল্লেখ আছে। HTTPS transit protection-কে endpoint/log exposure-এর সমাধান হিসেবে অতিরঞ্জিত করা হয়নি.
2. **Layered controls:** Short lifetime-কে একমাত্র solution বলা হয়নি; least privilege, audience restriction, exposure reduction, revocation এবং প্রয়োজনমতো sender-constraining-এর ভূমিকা আলাদা করা হয়েছে.
3. **Password change/logout narrative:** Password change বা client-side logout যে সব architecture-এ already-issued token স্বয়ংক্রিয়ভাবে invalidate করে না, opening ও পরের আলোচনা তা স্পষ্ট করে.
4. **Incident attribution:** IP, timestamp এবং `sub` claim-কে একা মানুষের পরিচয়ের প্রমাণ ধরা হয়নি.
5. **Scope control:** Chapter 12 refresh-token rotation/reuse detection, logout recovery এবং OAuth normative flow নিজের মধ্যে টেনে নেয়নি। Chapter 13 ও timeline-এর সঙ্গে scope সামঞ্জস্যপূর্ণ.

### Outcome

- Technical Review: **Complete**, source-based conceptual review.
- Security Review: **Complete**, threat-model-oriented conceptual review.
- Blocking technical/security defect found in this pass: **None**.
- Article rewrite: **Not needed**; বর্তমান wording-এর জন্য অপ্রয়োজনীয় rewrite এড়ানো হয়েছে.
- Authoritative sources rechecked: [RFC 7519](https://www.rfc-editor.org/rfc/rfc7519.html), [RFC 8725](https://www.rfc-editor.org/rfc/rfc8725.html), [RFC 9449](https://www.rfc-editor.org/rfc/rfc9449.html), [RFC 8705](https://www.rfc-editor.org/rfc/rfc8705.html). Existing references to RFC 6750, RFC 7009 and the OWASP OAuth 2.0 Cheat Sheet were also reviewed in context.

### Limitations

এটি article-এর source-based conceptual review, production implementation audit নয়। কোনো application/mobile/browser test suite, automated link checker বা Mermaid renderer চালানো হয়নি। তাই relative links ও Mermaid diagram-এর rendered output-কে automatedভাবে verify করা হয়েছে বলে দাবি করা হচ্ছে না.
