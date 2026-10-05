# Browser security

Browser-এর আচরণ আর cookie-র স্পেসিফিকেশন দুটোই বদলায়। কোনো ডিফল্টের উপর অধ্যায় দাঁড়ালে — যেমন `SameSite`-এর ডিফল্ট — লেখার সময়ের MDN আর তখনকার cookie RFC বা ড্রাফট দেখে নাও। কোনো ডিফল্টকে product ডকুমেন্টে স্থায়ী সত্য বানিয়ে রেখো না।

## Cookies

Browser ছোট ডেটা জমিয়ে রাখে, আর পরের request-এর সঙ্গে পাঠাতে পারে। KenaKata-র প্রথম login এই জিনিসটার উপর দাঁড়ায়।

- কোন অধ্যায়: ৪, ৫, ৬, ৭। SPA-তে token রাখার অধ্যায় ১০-এও browser কী পাঠায় সেটা কাজে লাগে।
- কেন গুরুত্বপূর্ণ: Session cookie না বুঝলে পরের আক্রমণগুলোর ছবি আলাদা হয় না।
- [MDN: HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
- [MDN: Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)
- [RFC 6265 — HTTP State Management Mechanism](https://www.rfc-editor.org/rfc/rfc6265)

RFC 6265 প্রকাশিত cookie RFC। যে সংশোধন এখনো ড্রাফট, সেটাকে ড্রাফট বলেই উদ্ধৃত করবে:

- [draft-ietf-httpbis-rfc6265bis — Cookies: HTTP State Management Mechanism](https://httpwg.org/http-extensions/draft-ietf-httpbis-rfc6265bis.html)

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
