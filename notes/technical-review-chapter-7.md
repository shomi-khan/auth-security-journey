# Chapter 7: CORS Technical ও Security Review

**Date:** 2026-10-25  
**Branch:** `docs/chapters-11-25-blueprints`  
**Review type:** Source-based conceptual review. এটি browser compatibility test, application audit বা production security certification নয়।

## Review coverage

- নতুন Chapter 7-এর সম্পূর্ণ draft
- আগের Chapter 6 (CSRF)
- পরের Chapter 8 (XSS এবং HttpOnly)
- `references/browser-security.md`, `references/owasp.md` এবং `product/timeline.md`
- CORS-এর মূল behavior-এর জন্য MDN CORS guide ও WHATWG Fetch Standard

## Technical findings

1. **Origin বনাম site:** Origin-এ scheme, host ও port থাকে। একই site-এর দুই origin আলাদা origin হতে পারে, এই পার্থক্য স্পষ্ট।
2. **SOP ও CORS:** CORS-কে browser-এর cross-origin response-sharing mechanism হিসেবে বর্ণনা করা হয়েছে; API authorization হিসেবে নয়।
3. **Simple request বনাম preflight:** সব cross-origin request-এর আগে preflight হয় না। Preflighted request-এ অনুমতি না পেলে browser actual request পাঠায় না; simple request server-এ পৌঁছে যেতে পারে।
4. **Response headers:** `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`, `Access-Control-Allow-Credentials` এবং dynamic origin response-এ `Vary: Origin`-এর ভূমিকা যথাযথভাবে সীমাবদ্ধ করা হয়েছে।
5. **Credentialed CORS:** Credential mode ও cookie policy-র পার্থক্য বলা হয়েছে; credentialed response-এ wildcard `Access-Control-Allow-Origin: *` ব্যবহার করা যায় না।
6. **Origin matching:** Exact scheme/host/port allowlist, arbitrary Origin reflection এবং `Origin: null`-কে নির্বিচারে trust না করার কথা আছে।

## Security findings

1. **CORS ≠ authentication/authorization:** Origin user identity প্রমাণ করে না। API-তে authentication, function-level authorization এবং resource-level authorization আলাদা থাকে।
2. **CORS ≠ complete CSRF defense:** Simple cross-origin request server-এ পৌঁছাতে পারে এবং response না পড়েও state change ঘটতে পারে। Chapter 6-এর CSRF controls আলাদা রাখা হয়েছে।
3. **Non-browser clients:** CORS browser-enforced; অন্য client নিজস্বভাবে Origin header পাঠাতে পারে, তাই server-side permission CORS-এর ওপর নির্ভর করতে পারে না।
4. **Trusted origin risk:** Allowlist-এ থাকা frontend compromised হলে credentialed response exposure-এর ঝুঁকি থাকতে পারে।
5. **Continuity:** Chapter 7-এর scenario generic browser/API example হিসেবে চিহ্নিত, যাতে KenaKata-র single-web-application timeline-এ অঘোষিত architecture change না আসে। Chapter 6-এর পরে এবং Chapter 8 XSS-এর আগে transition স্পষ্ট।

## Outcome

- Technical Review: **Complete**, source-based conceptual review.
- Security Review: **Complete**, threat-model-oriented conceptual review.
- Blocking conceptual defect found: **None in this pass**.
- Mermaid: Source text reviewed; renderer চালানো হয়নি।
- Browser, automated test suite, application configuration বা production deployment পরীক্ষা করা হয়নি। এটি production security audit বা certification নয়।

## Authoritative sources

- [MDN: Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [WHATWG Fetch Standard: CORS protocol](https://fetch.spec.whatwg.org/#http-cors-protocol)
- [MDN: Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
- [OWASP: Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
