# Chapter 8 লেখার নোট: Token কেন দরকার?

এই ফাইল লেখার আগে scope ঠিক করার জন্য। Chapter-এর অংশ নয়।

## Product timeline

- KenaKata এখন mobile app যোগ করছে, মালিক ও কর্মীরা ফোন থেকে দোকান চালাবে।
- পুরোনো server-rendered web app cookie-backed session ব্যবহার করতে থাকবে। Mobile app আসার অর্থ website-এর session সরিয়ে ফেলা নয়।
- Mobile app access token এবং refresh token ব্যবহার করবে। Refresh token-এর বিস্তারিত Chapter 12–14-এ; Chapter 8-এ শুধু জানানো যাবে যে পরে access token নতুন করে পাওয়ার আলাদা mechanism লাগবে।
- SPA এখনো আসেনি। Browser-এ token কোথায় রাখা হবে, সেটা Chapter 10-এর scope।
- OAuth, PKCE, OIDC এবং third-party integration এখনো KenaKata-তে চালু হয়নি। OAuth protocol flow Chapter 16–21-এ শেখানো হবে। Chapter 8-এ login-কে OAuth authorization-code flow হিসেবে দেখাবে না।

## কেন্দ্রীয় প্রশ্ন

Mobile app যখন KenaKata-র API-তে request পাঠায়, server কীভাবে বুঝবে request-টি কোন user-এর এবং কোন access grant-এর অধীনে করা হচ্ছে, যদি browser-এর cookie-session context-টাই client model না হয়?

Token হলো credential/representation, জাদুকরী নিরাপত্তা নয়। Token ব্যবহার করার কারণ client-এর request-এ credential বহনের একটি model দরকার; token হওয়ায় সেটি নিজে থেকে cookie-এর চেয়ে নিরাপদ হয়ে যায় না।

## Chapter 8-এ ব্যাখ্যা করতে হবে

1. **Cookie session বনাম access token:** server-side session model-এ browser cookie session identifier বহন করে; access-token model-এ client API request-এর সঙ্গে access token পাঠায়। দুটো model-এর boundary পরিষ্কার করতে হবে, কোনো একটিকে সব পরিস্থিতিতে superior বলা যাবে না।
2. **কেন mobile-এ token:** mobile app নিজস্ব client, যা KenaKata-র API-তে request পাঠায়। Server-কে প্রতিটি request-এর credential যাচাই করতে হবে। Browser cookie একমাত্র সম্ভাব্য পদ্ধতি নয়; “mobile app cookie ব্যবহার করতে পারে না” এমন absolute claim করা যাবে না।
3. **Illustrative request:** HTTPS-এ API request-এ Authorization: Bearer <access-token> header-এর উদাহরণ। Token URL/query string-এ দেবে না; logs, crash reports বা analytics-এ token ফাঁস হওয়ার ঝুঁকি উল্লেখ করবে।
4. **Bearer property:** এই bearer-token model-এ token যার হাতে আছে, সে token-এর অনুমোদিত সীমার মধ্যে সেটি ব্যবহার করার চেষ্টা করতে পারে। Token possession নিজে user-এর সব resource-এ access দেয় না; resource server token validity, intended audience/target, applicable scope/permission এবং resource authorization যাচাই করবে।
5. **Failure path:** token নেই বা invalid হলে request authenticate হবে না; valid token থাকলেও permission না থাকলে operation reject হবে। Token expired/revoked হওয়ার বিস্তারিত Chapter 11–14-এ, এখানে শুধু boundary ও future bridge।
6. **Trade-off:** password প্রতিটি API request-এ পাঠানোর বদলে client short-lived credential বহন করতে পারে, কিন্তু token চুরি হলে replay হতে পারে। Token secure transport ও safe storage-এ রাখতে হয়; HTTPS ছাড়া bearer token পাঠানো যাবে না।
7. **Security invariant:** “Credential গ্রহণ করলেই operation অনুমোদিত হয় না; প্রতিটি protected resource request-এ token validity এবং operation-এর authorization আলাদা করে যাচাই করতে হবে।”

## Chapter 8-এ এখনই শেখাবে না

- JWT-এর header/payload/signature, claim validation বা signing algorithm: Chapter 9।
- Browser localStorage/sessionStorage, cookie/BFF trade-off, SPA token storage: Chapter 10।
- Expiration, replay timeline ও attack window-এর বিস্তারিত: Chapter 11।
- Refresh token lifecycle, rotation, reuse detection, revocation: Chapter 12–14।
- OAuth authorization endpoint, authorization code, PKCE, OIDC, identity token: Chapter 16–21।
- DPoP, sender-constrained token, proof-of-possession: ভবিষ্যৎ অধ্যায় কেবল threat model প্রয়োজন করলে।
- Language-specific implementation code, runnable commands, framework-specific SDK instructions.

## Diagrams

- Mermaid sequence diagram: Mobile App → KenaKata API, token-সহ request, server-side validation, authorization decision, success/401/403 failure path।
- ছোট comparison table: cookie-backed web session বনাম mobile access-token request model।
- HTTP request/response illustrative example allowed; কোনো programming language implementation নয়।

## Source plan

- RFC 6750, Sections 1–2 and 5: bearer token ধারণা, Authorization: Bearer header, token disclosure/replay risk, token URL-এ না দেওয়ার কারণ। RFC 6750 OAuth bearer-token usage standard; Chapter 8-এ এটি bearer-token model বোঝাতে ব্যবহার করবে, কিন্তু KenaKata-র first-party mobile login-কে OAuth flow বলে দাবি করবে না।
- RFC 6749: access token OAuth framework-এর শব্দ হিসেবে ব্যবহৃত হলে definition-এর জন্য; OAuth protocol mechanics Chapter 16-এর আগে আনবে না।
- OWASP guidance: access token-কে sensitive credential হিসেবে সামলানো, logs/URLs-এ না ফেলা ও transport protection-এর দাবিগুলো লেখার আগে প্রাসঙ্গিক বর্তমান official guidance দেখে নিতে হবে।
- Sources-এ যা স্পষ্টভাবে বলা আছে শুধু সেটাই claim হিসেবে ব্যবহার করবে। Token format বা token validation-এর implementation assumptions বানাবে না।

## Review checklist

- [ ] Chapter 8-এ mobile app timeline-এ সঠিক স্থানে আছে।
- [ ] Existing web cookie session মুছে ফেলা হয়নি।
- [ ] Token-কে cookie-র চেয়ে intrinsically safer বলা হয়নি।
- [ ] OAuth/PKCE/OIDC flow Chapter 8-এ আগেভাগে ঢোকেনি।
- [ ] Bearer-token theft/replay risk বলা হয়েছে, কিন্তু expiry/revocation-এর পরের অধ্যায়গুলো পুনরায় লেখা হয়নি।
- [ ] Authentication, token validation ও authorization আলাদা।
- [ ] Failure path এবং security invariant দৃশ্যমান।
- [ ] No language-specific implementation code.
- [ ] Technical Review ও Security Review claims/source যাচাইয়ের পরই mark হবে; browser/app tests না করলে সেগুলো চালানো হয়েছে এমন দাবি করা হবে না.