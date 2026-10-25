---
chapter: 16
season: 2
title: "OAuth 2.0 কেন?"
status: planned
---

# OAuth 2.0 কেন?

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
Season 1-এ KenaKata নিজের user ও নিজের client-এর credential সামলেছে। এবার কোনো third-party application-কে user-এর password না দিয়ে, নির্দিষ্ট সীমার access দেওয়ার সমস্যা আসবে। OAuth 2.0 কী সমস্যা সমাধান করে এবং কী সমাধান করে না, সেই mental model তৈরি হবে।

## গল্পের শুরু
একটি হিসাবরক্ষার app রাফির KenaKata order থেকে মাসিক report তৈরি করতে চায়। আরিফ প্রথমে app-টিকে রাফির password দিতে বলে। নীরা থামায়: app-এর password জানার দরকার নেই, আর order-এর সব কাজের অনুমতিও তার দরকার নেই।

## আলোচ্য বিষয়
- Delegated authorization-এর প্রয়োজন
- Resource owner, client, authorization server ও resource server-এর ভূমিকা
- Access token কী বোঝায় এবং scope/permission-এর সীমা
- OAuth 2.0 authorization framework, authentication protocol নয়
- First-party login বনাম third-party delegated access
- High-level Authorization Code Flow-এর actor ও trust boundary
- Confidential client বনাম public client-এর ধারণা
- OAuth flow-এ redirect-এর ভূমিকা এবং browser/app-এর সীমাবদ্ধতা
- Protocol-এর নিয়ম, security best practice এবং product-specific design decision আলাদা করে চেনা

## অধ্যায় শেষে পাঠক পারবে
- OAuth দরকার এমন ও দরকার নেই এমন use case আলাদা করতে
- OAuth-এর প্রধান actor-দের দায়িত্ব বোঝাতে
- OAuth access token দিয়ে user authentication নিশ্চিত হয়ে যায়, এমন ভুল এড়াতে
- পরের অধ্যায়গুলোর flow diagram পড়ার জন্য trust boundary চিহ্নিত করতে

## এই অধ্যায়ে থাকবে না
- state-এর বিস্তারিত threat analysis; অধ্যায় ১৭
- Redirect URI ও code theft-এর বিস্তারিত; অধ্যায় ১৮
- PKCE; অধ্যায় ১৯
- OIDC ও Google login; অধ্যায় ২০–২১

## ধারাবাহিকতা
আগে: [অধ্যায় ১৫: IDOR/BOLA এবং Authorization Design](../../season-1-browser-session-token/15-idor-bola-authorization/README.md)

পরে: [অধ্যায় ১৭: Authorization Code Flow এবং state](../17-authorization-code-state/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
