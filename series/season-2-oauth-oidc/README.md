# Season 2 — OAuth, OIDC এবং Delegated Identity

অধ্যায় ১৬–২১। একটাও এখনো লেখা হয়নি।

Product-এর তথ্য [product/](../../product/README.md)-এ। Google login আর বাইরের application কখন চালু হয়, সেটা [timeline](../../product/timeline.md)-এ। এই season-এর শুরুতে দুটোই এখনো দাবি, চালু feature নয়।

## এই Season কেন পড়ব?

Season 1-এ KenaKata নিজের password আর নিজের token সামলায়। এবার অন্য প্রশ্ন। কোনো app-কে দোকানের password না দিয়ে সীমিত access দিতে হয়। আবার user চায় Google দিয়ে ঢুকতে।

OAuth 2.0 আর OIDC এই দুই দাবির ভাষা। Google login আলাদা জাদু নয়। ওই ডিজাইনের একটা ব্যবহার।

## কী কী সমস্যা solve করব?

- Password না দিয়ে অন্য app-কে access দেওয়া
- মাঝপথে code চুরি হওয়া, আর redirect ঠিকানা ভুল হওয়া
- Mobile বা SPA-র মতো client, যে নিজে secret লুকিয়ে রাখতে পারে না
- Access পাওয়া আর user কে সেটা জানা — দুটোকে এক করে ফেলা

## কোন concepts শিখব?

OAuth, Authorization Code, `state`, redirect URI, PKCE, OIDC, ID Token, Access Token, Google Login।

## Chapter list

16. [OAuth 2.0 কেন?](16-oauth/README.md)
17. [Authorization Code Flow এবং `state`](17-authorization-code-state/README.md)
18. [Redirect URI এবং Authorization Code Theft](18-redirect-uri-code-theft/README.md)
19. [PKCE](19-pkce/README.md)
20. [OIDC](20-oidc/README.md)
21. [Google Login এবং ID Token বনাম Access Token](21-google-login/README.md)

## Season শেষে reader কী বুঝতে পারবে?

- OAuth 2.0 দিয়ে সীমিত access দেওয়া যায়। এটা একা “user কে” বলে দেয় না
- `state`, redirect URI, আর PKCE কেন আলাদা আলাদা সমস্যার জন্য
- ID token কী যোগ করে, আর access token থেকে সেটা আলাদা কোথায়
- Google একটা external identity provider। বাইরের application KenaKata-র OAuth client

[সিরিজের সূচি](../../README.md)
