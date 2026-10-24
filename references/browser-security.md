# Browser security

Browser-এর আচরণ আর cookie-র স্পেসিফিকেশন দুটোই বদলায়। কোনো ডিফল্টের উপর অধ্যায় দাঁড়ালে — যেমন `SameSite`-এর ডিফল্ট — লেখার সময়ের MDN আর তখনকার cookie RFC বা ড্রাফট দেখে নাও। কোনো ডিফল্টকে product ডকুমেন্টে স্থায়ী সত্য বানিয়ে রেখো না।

## Cookies

Browser ছোট ডেটা জমিয়ে রাখে, আর পরের request-এর সঙ্গে পাঠাতে পারে। KenaKata-র প্রথম login এই জিনিসটার উপর দাঁড়ায়।

- কোন অধ্যায়: ৪, ৫, ৬, ৭। SPA-তে token রাখার অধ্যায় ১০-এও browser কী পাঠায় সেটা কাজে লাগে।
- কেন গুরুত্বপূর্ণ: Session cookie না বুঝলে পরের আক্রমণগুলোর ছবি আলাদা হয় না।
- [MDN: HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
- [MDN: Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)
- [RFC 10025 — Cookies: HTTP State Management Mechanism](https://www.rfc-editor.org/info/rfc10025)
- [RFC 6265 — historical cookie standard](https://www.rfc-editor.org/info/rfc6265)

RFC 10025 জুলাই ২০২৬-এ প্রকাশিত হয়ে RFC 6265-কে supersede করেছে। Cookie-সংক্রান্ত বর্তমান normative claim-এ RFC 10025 ব্যবহার করো; RFC 6265-কে ঐতিহাসিক ব্যাখ্যার জন্য উদ্ধৃত করতে পারো। প্রকাশের আগে RFC Editor-এর বর্তমান status আবার যাচাই করো।

## Secure

`Secure` একটা `Set-Cookie` অ্যাট্রিবিউট। মানে উপরের cookie ডকুমেন্টে। আলাদা product-নিয়ম নয়।

- কোন অধ্যায়: ৪ আর ৫।
- কেন গুরুত্বপূর্ণ: Cookie কোন সংযোগে যাবে, সেই প্রশ্নে এটা আসে। কখন যায়, সোর্স পড়ে লিখবে।

## HttpOnly

`HttpOnly` একটা `Set-Cookie` অ্যাট্রিবিউট। অধ্যায় ৭ এটা নিয়ে। Flag কী সরিয়ে দেয় আর কোন আক্রমণ থামিয়ে দেয় না, সেটা অধ্যায়ে সোর্স দেখে লিখতে হবে। “HttpOnly থাকলে XSS সমস্যা নেই” এই ধারণা [ভুল ধারণার তালিকায়](../notes/misconceptions.md) আছে। এখানে সেটার রায় দেওয়া হয়নি।

- কোন অধ্যায়: ৭।
- কেন গুরুত্বপূর্ণ: XSS আর cookie একসঙ্গে পড়ার সময় এই flag না দেখলে ছবি আধা হয়।

## SameSite

`SameSite` একটা `Set-Cookie` অ্যাট্রিবিউট। বর্তমান `Set-Cookie` ডকুমেন্ট আর উপরের cookie ড্রাফট পড়ো। একটা browser-এর ডিফল্টকে অ্যাট্রিবিউটের মানে বানিয়ে ফেলবে না।

- কোন অধ্যায়: ৬, আর session-এর অধ্যায় ৪–৫।
- কেন গুরুত্বপূর্ণ: CSRF-এর আলোচনায় এটা আসে। কতদূর আটকায়, সেটা লেখার সময়ের সোর্স বলবে।

## Origin

Browser কোন পাতা থেকে request গেছে, সেই সীমানা CSRF, XSS, আর CORS-এর আলোচনায় লাগে।

- কোন অধ্যায়: ৬ আর ৭।
- কেন গুরুত্বপূর্ণ: “একই সাইট” আর “অন্য সাইট” না আলাদা করলে cookie কখন যায় সেটা বোঝা যায় না।
- [MDN: Origin header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin)
- [MDN: Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
- [HTML Living Standard: Origin](https://html.spec.whatwg.org/multipage/browsers.html#origin)

## Fetch Metadata এবং CSRF

- কোন অধ্যায়: ৬; পরে ১০-এ browser client-এর request context বোঝার সময়।
- কেন গুরুত্বপূর্ণ: `Sec-Fetch-Site` request initiator ও target-এর site/origin সম্পর্ক জানায়। এটি browser-সৃষ্ট signal, authentication নয়; non-browser client নিজে header লিখতে পারে।
- [W3C: Fetch Metadata Request Headers](https://www.w3.org/TR/fetch-metadata/): `Sec-Fetch-Site`-এর মান ও security considerations। ২১ সেপ্টেম্বর ২০২৬-এর সংস্করণ Working Draft, Recommendation নয়।
- [MDN: Fetch metadata](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Fetch_metadata)
- [WHATWG: Fetch Standard, forbidden request-header names](https://fetch.spec.whatwg.org/): `Sec-` prefix-যুক্ত header browser API থেকে set/modify করা যায় না।
- [RFC 9110 §9.2.1: Safe Methods](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.1): safe method-এর semantics; `GET`-এ state change করা যাবে না।
- [RFC 7617 §2.2: Reusing Credentials](https://www.rfc-editor.org/rfc/rfc7617.html#section-2.2): HTTP Basic-এর credential reuse ও preemptive sending-এর protocol rule; নির্দিষ্ট browser behavior আলাদাভাবে যাচাই করতে হবে।
- [Go `net/http` `CrossOriginProtection`](https://pkg.go.dev/net/http@go1.25#CrossOriginProtection): Go 1.25-এর optional implementation reference। Go 1.25 documentation অনুযায়ী `Sec-Fetch-Site` ও `Origin` দুটোই অনুপস্থিত হলে request allow করা হয়; KenaKata-র fail-safe policy এর থেকে ইচ্ছাকৃতভাবে কঠোর। ব্যবহার করলে current Go version ও default behavior যাচাই করো।
- [Go vulnerability report GO-2025-3955](https://pkg.go.dev/vuln/GO-2025-3955): `CrossOriginProtection.AddInsecureBypassPattern`-এর নির্দিষ্ট issue, Go 1.25.0-এ প্রভাবিত এবং 1.25.1-এ সংশোধিত।

KenaKata-র Chapter 6 policy হলো: state-changing request-এ `Sec-Fetch-Site` অনুপস্থিত থাকলে configured target origin-এর সঙ্গে `Origin` হুবহু মেলানো হবে; দুটো signal-ই অনুপস্থিত বা `Origin: null` হলে fail-safe reject। `same-site` ও `none`-কেও sensitive state-changing action-এর জন্য নিজে থেকে trust করা হবে না। এটি KenaKata-র threat-model decision, browser standard-এর universal requirement নয়।

## CORS, যেখানে দরকার

CORS browser-এর নিয়ম: অন্য origin-এর response পড়া যাবে কি না। এটা KenaKata-র API-এর authorization নয়।

- কোন অধ্যায়: ১০, যখন SPA আলাদা origin থেকে API ডাকতে পারে। পরে API-র অধ্যায়েও।
- কেন গুরুত্বপূর্ণ: CORS header বসালেই কে কোন resource দেখতে পাবে, সেই প্রশ্ন শেষ হয় না। দুটোকে এক করে লিখবে না।
- [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Fetch Living Standard](https://fetch.spec.whatwg.org/)

## Browser credential behavior

Cookie কখন request-এর সঙ্গে যায়, আর `fetch` বা navigation credential পাঠায় কি না, সেটা browser ঠিক করে। `SameSite`, `Secure`, origin, আর request কীভাবে পাঠানো হয়েছে — এগুলো একসঙ্গে দেখতে হয়।

- কোন অধ্যায়: ৪, ৬, আর ১০।
- কেন গুরুত্বপূর্ণ: “Cookie থাকলেই পরের request-এ যাবে” — এই কথাটা ডিফল্ট হিসেবে লিখে রেখো না। লেখার সময় MDN আর cookie ড্রাফট দেখে নাও।
- [MDN: HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

## Session Fixation testing

- কোন অধ্যায়: ৫।
- কেন পড়া useful: login-এর আগে জানা session ID-তে login করানো যায় কি না এবং login-এর সময় ID rotate হচ্ছে কি না, তা যাচাই করার test guidance।
- [OWASP WSTG-SESS-03: Session Fixation, latest development guide](https://wstg.owasp.org/latest/4-Web_Application_Security_Testing/06-Session_Management/03-Session_Fixation/)
- [OWASP WSTG v4.2: Testing for Session Fixation, stable version](https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation/)

## Device Bound Session Credentials (DBSC)

- কোন অধ্যায়: ৫।
- কেন পড়া useful: DBSC কীভাবে device-bound key দিয়ে session cookie refresh সুরক্ষিত করার চেষ্টা করে, আর ongoing compromised device-এর মতো কোন ঝুঁকি এটি সমাধান করে না, তা বোঝার জন্য।
- [W3C WebAppSec DBSC explainer](https://github.com/w3c/webappsec-dbsc/blob/main/README.md)
- [W3C DBSC Editor's Draft](https://w3c.github.io/webappsec-dbsc/)
- [Chrome Developers: DBSC now available on Windows](https://developer.chrome.com/blog/dbsc-windows-announcement)

DBSC-র specification ও browser support পরিবর্তনশীল। প্রকাশের আগে W3C status এবং browser vendor-এর বর্তমান documentation আবার যাচাই করো।


## SPA token storage এবং BFF

- কোন অধ্যায়: ১০।
- কেন গুরুত্বপূর্ণ: Browser storage API-তে থাকা token-এ একই origin-এর JavaScript access পেতে পারে; cookie browser নিজে request-এ যোগ করতে পারে; BFF token server-side-এ রাখতে পারে, কিন্তু XSS/CSRF threat পুরোপুরি সরায় না।
- [RFC 10017 — OAuth 2.0 for Browser-Based Applications](https://www.rfc-editor.org/rfc/rfc10017.html): browser-based OAuth architecture ও token-storage threat model। এটি Internet Best Current Practice, আগস্ট ২০২৬।
- [MDN: Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API): localStorage ও sessionStorage-এর lifetime ও scope।
- [MDN: Secure cookie configuration](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/Cookies): HttpOnly, Secure, SameSite ও cookie prefix।
