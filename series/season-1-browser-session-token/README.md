# Season 1 — Browser, Session এবং Token

অধ্যায় ১–১৫। একটাও এখনো লেখা হয়নি।

Product-এর তথ্য [product/](../../product/README.md)-এ। কোন অধ্যায়ে কী আগে থেকে আছে, সেটা [timeline](../../product/timeline.md)-এ।

## এই Season কেন পড়ব?

KenaKata একটা ছোট দোকানের website। কেউ login করল। পরের request-টাও তার, সেটা server কীভাবে বুঝবে?

এই season সেই প্রশ্ন থেকে শুরু করে। Cookie-তে session, সেই session ভাঙলে কী হয়, mobile এলে কেন token লাগে, আর শেষে “login করা আছে” মানেই সব resource-এ access নয়।

## কী কী সমস্যা solve করব?

- Login-এর পর password বারবার না দিয়ে একই মানুষকে চেনা
- Cookie অন্যের হাতে গেলে, বা অন্য সাইট থেকে request এলে কী হয়
- Mobile আর SPA এলে শুধু browser cookie আর যথেষ্ট না হওয়া
- Logout করলে কী বাতিল হয়
- রাফির দোকানের order অন্য দোকান দেখে ফেলা

সমাধানগুলো শেষ কথা নয়। প্রতিটা সমাধানের পর trade-off থাকবে।

## কোন concepts শিখব?

Authentication, Authorization, Password, Session, Cookie, CSRF, XSS, Token, JWT, Access Token, Refresh Token, Revocation, IDOR/BOLA।

## Chapter list

1. [কেন Authentication দরকার?](01-why-authentication/README.md)
2. [Authentication বনাম Authorization](02-authentication-vs-authorization/README.md)
3. [প্রথম Login System এবং Password Security](03-password-security/README.md)
4. [Session এবং Cookie](04-session-and-cookie/README.md)
5. [Session Hijacking এবং Session Fixation](05-session-hijacking-fixation/README.md)
6. [CSRF](06-csrf/README.md)
7. [XSS এবং HttpOnly-এর সীমাবদ্ধতা](07-xss-and-httponly/README.md)
8. [Token কেন দরকার?](08-tokens/README.md)
9. [JWT আসলে কী?](09-jwt/README.md)
10. [SPA-তে Token কোথায় রাখব?](10-spa-token-storage/README.md)
11. [Token Expiration এবং Replay Window](11-token-expiration-replay/README.md)
12. [Refresh Token কেন?](12-refresh-token/README.md)
13. [Refresh Token Rotation এবং Reuse Detection](13-refresh-token-rotation/README.md)
14. [Logout এবং Revocation](14-logout-revocation/README.md)
15. [IDOR/BOLA এবং Authorization Design](15-idor-bola-authorization/README.md)

## Season শেষে reader কী বুঝতে পারবে?

- Authentication আর Authorization আলাদা প্রশ্ন
- Cookie-তে রাখা session কী, আর সেটা কীভাবে ভাঙতে পারে — চুরি, fixation, CSRF, XSS এক জিনিস নয়
- Access token আর refresh token আলাদা। রাখা, মেয়াদ, rotation, আর বাতিল করা আলাদা আলাদা প্রশ্ন
- Login থাকা, বা token-এ role লেখা থাকা, মানেই ওই resource-এ access আছে — এটা ধরে নেওয়া যাবে না

[সিরিজের সূচি](../../README.md)
