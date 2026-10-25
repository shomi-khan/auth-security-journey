---
chapter: 14
season: 1
title: "Logout, Revocation এবং Session Recovery"
status: planned
---

# Logout, Revocation এবং Session Recovery

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
Logout button চাপলে আসলে কী শেষ হয়? Client-এর local state মুছে ফেলা, server-side session বা refresh token revoke করা, আর self-contained access token-এর কার্যকর মেয়াদ শেষ হওয়া এক ঘটনা নয়। এই অধ্যায় এসব পার্থক্য এবং incident-এর পর recovery policy ব্যাখ্যা করবে।

## গল্পের শুরু
রাফি mobile app থেকে logout করে। UI login screen-এ ফিরে যায়, কিন্তু আরিফ জানতে পারে পুরোনো access token কিছু সময় resource API-তে কাজ করতে পারে। পরে এক কর্মীর ফোন হারিয়ে যায়, আর নীরাকে ঠিক করতে হয় কী বাতিল করা দরকার এবং অন্য device-গুলোতে কী হবে।

## আলোচ্য বিষয়
- Client-side logout: local credential/state পরিষ্কার করা
- Server-side revocation: session, refresh token বা token family নিষ্ক্রিয় করা
- Opaque token, introspection এবং self-contained JWT access token-এর revocation trade-off
- Short lifetime, denylist এবং server-side state-এর খরচ ও সীমা
- Logout from one device বনাম logout all devices
- Password reset, account compromise এবং employee offboarding-এর recovery flow
- Refresh token বাতিল হলেও ইতিমধ্যে ইস্যু করা access token কেন সঙ্গে সঙ্গে অকার্যকর নাও হতে পারে
- Audit log, user notification এবং incident response-এর প্রয়োজন
- “Logout সফল” বলে UI দেখানো আর সব credential revoke হওয়ার নিশ্চয়তার পার্থক্য

## অধ্যায় শেষে পাঠক পারবে
- Local logout, expiry ও revocation আলাদা করে ব্যাখ্যা করতে
- JWT access token তাৎক্ষণিক revoke করা কঠিন কেন তা বলতে
- হারানো device বা compromised account-এর জন্য একটি recovery policy প্রস্তাব করতে
- Security, availability, latency ও operational complexity-এর trade-off তুলনা করতে

## এই অধ্যায়ে থাকবে না
- OAuth token revocation endpoint-এর পূর্ণ protocol specification
- MFA বা step-up authentication; অধ্যায় ২২
- নির্দিষ্ট vendor বা framework-এর implementation

## ধারাবাহিকতা
আগে: [অধ্যায় ১৩: Refresh Token Rotation এবং Reuse Detection](../13-refresh-token-rotation/README.md)

পরে: [অধ্যায় ১৫: IDOR/BOLA এবং Authorization Design](../15-idor-bola-authorization/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
