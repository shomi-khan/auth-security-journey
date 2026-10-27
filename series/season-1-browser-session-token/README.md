# Season 1 — Browser, Session এবং Token

অধ্যায় ১–১৬-এর draft আছে। কোনো অধ্যায় এখনো Published নয়.

Product-এর তথ্য/ [product/](../../product/README.md)-এ। কোন অধ্যায়ে কী আগে থেকে আছে, সেটা [timeline](../../product/timeline.md)-এ।

## এই Season কেন পড়ব?

KenaKata একটা ছোট দোকানের website। কেউ login করল। পরের request-টাও তার, সেটা server কীভাবে বুঝবে?

এই season সেই প্রশ্ন থেকে শুরু করে। Cookie-তে session, সেই session ভাঙলে কী হয়, browser cross-origin response পড়ার নিয়ম, mobile এলে কেন token লাগে, আর শেষে “login করা আছে” মানেই সব resource-এ access নয়।

## কী কী সমস্যা solve করব?

- Login-এর পর password বারবার না দিয়ে একই মানুষকে চেনা
- Cookie অন্যের হাতে গেলে, বা অন্য সাইট থেকে request এলে কী হয়
- Browser অন্য origin-এর API response কেন পড়তে পারে না
- Mobile আর SPA এলে শুধু browser cookie আর যথেষ্ট না হওয়া
- Logout করলে কী বাতিল হয়
- রাফির দোকানের order অন্য দোকান দেখে ফেলা

সমাধানগুলো শেষ কথা নয়। প্রতিটা সমাধানের পর trade-off থাকবে।

## কোন concepts শিখব?

Authentication, Authorization, Password, Session, Cookie, CSRF, CORS, XSS, Token, JWT, Access Token, Refresh Token, Revocation, IDOR/BOLA।

## Chapter list

1. [কেন Authentication দরকার?](01-why-authentication/README.md)
2. [Authentication বনাম Authorization](02-authentication-vs-authorization/README.md)
3. [প্রথম Login System এবং Password Security](03-password-security/README.md)
4. [Session এবং Cookie](04-session-and-cookie/README.md)
5. [Session Hijacking এবং Session Fixation](05-session-hijacking-fixation/README.md)
6. [CSRF](06-csrf/README.md)
7. [CORS: Browser কোন Response পড়তে পারবে?](07-cors/README.md)
8. [XSS এবং HttpOnly-এর সীমাবদ্ধতা](08-xss-and-httponly/README.md)
9. [Token কেন দরকার?](09-tokens/README.md)
10. [JWT আসলে কী?](10-jwt/README.md)
11. [SPA-তে Token কোথায় রাখব?](11-spa-token-storage/README.md)
12. [Token Expiration এবং Replay Window](12-token-expiration-replay/README.md)
13. [Refresh Token কেন?](13-refresh-token/README.md)
14. [Refresh Token Rotation এবং Reuse Detection](14-refresh-token-rotation/README.md)
15. [Logout, Revocation এবং Session Recovery](15-logout-revocation/README.md)
16. [IDOR/BOLA এবং Authorization Design](16-idor-bola-authorization/README.md)

## Season শেষে reader কী বুঝতে পারবে?

- Authentication আর Authorization আলাদা প্রশ্ন
- Cookie-তে রাখা session কী, আর সেটা কীভাবে ভাঙতে পারে: চুরি, fixation, CSRF, XSS এক জিনিস নয়
- CORS browser-এর response-sharing policy; API authorization বা CSRF defense নয়
- Access token আর refresh token আলাদা। রাখা, মেয়াদ, rotation, আর বাতিল করা আলাদা আলাদা প্রশ্ন
- Login থাকা, বা token-এ role লেখা থাকা, মানেই ওই resource-এ access আছে, এটা ধরে নেওয়া যাবে না

[সিরিজের সূচি](../../README.md)
