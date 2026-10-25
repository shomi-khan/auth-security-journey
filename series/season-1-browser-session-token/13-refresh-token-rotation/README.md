---
chapter: 13
season: 1
title: "Refresh Token Rotation এবং Reuse Detection"
status: planned
---

# Refresh Token Rotation এবং Reuse Detection

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
অধ্যায় ১২-তে refresh token-এর lifecycle বোঝার পর এবার দেখব, দীর্ঘমেয়াদি credential চুরি হলে ক্ষতি সীমিত করতে rotation কীভাবে সাহায্য করতে পারে। পাশাপাশি reuse detection কেন গুরুত্বপূর্ণ, এবং প্রতিটি reuse-কে নিশ্চিত আক্রমণ ধরে নেওয়া কেন ভুল হতে পারে, তা ব্যাখ্যা করা হবে।

## গল্পের শুরু
আরিফ refresh token একবার ব্যবহার হওয়ার পর পুরোনো token বাতিল করে নতুন token দেওয়ার ব্যবস্থা করে। কয়েক দিন পর log-এ পুরোনো token আবার দেখা যায়। এটা কি token চুরি, নাকি mobile app-এর network timeout-এর পর retry? একই লক্ষণ থেকে দুই রকম ঘটনা হতে পারে।

## আলোচ্য বিষয়
- Rotation-এর আগে ও পরে token family-এর lifecycle
- পুরোনো refresh token invalid করা এবং নতুন token ইস্যু করা
- Reuse detection কী সংকেত দেয়, আর কী প্রমাণ করে না
- Token family revoke করার মতো response-এর সুবিধা ও ব্যবহারকারীর ওপর প্রভাব
- Concurrent refresh request, একাধিক app process/device এবং refresh race
- Server token rotate করেছে কিন্তু response client পায়নি: ambiguous timeout
- অন্ধভাবে পুরোনো token retry করলে false positive বা family revocation কেন ঘটতে পারে
- Serial refresh, recovery policy এবং নিরাপদে সমর্থিত হলে সীমিত grace/idempotency-এর trade-off
- Rotation একমাত্র পদ্ধতি নয়: sender-constrained token-এর মতো বিকল্পের উচ্চস্তরের পরিচয়
- RFC 9700-এর OAuth-specific guidance-কে KenaKata-র first-party flow-এর সঙ্গে গুলিয়ে না ফেলা

## অধ্যায় শেষে পাঠক পারবে
- Rotation ও reuse detection-এর উদ্দেশ্য আলাদা করে বলতে
- Token reuse-কে সম্ভাব্য compromise signal হিসেবে বিশ্লেষণ করতে, নিশ্চিত প্রমাণ হিসেবে নয়
- Network failure ও concurrent request-এর ঝুঁকি একটি sequence diagram-এ দেখাতে
- Detection policy-র security benefit ও false-positive cost ব্যাখ্যা করতে

## এই অধ্যায়ে থাকবে না
- OAuth-এর সব client type-এর জন্য rotation বাধ্যতামূলক, এমন দাবি
- সব system-এ প্রযোজ্য একক refresh-token lifetime
- OAuth Authorization Code Flow ও PKCE-এর পূর্ণ আলোচনা
- Framework-specific implementation

## ধারাবাহিকতা
আগে: [অধ্যায় ১২: Refresh Token কেন?](../12-refresh-token/README.md)

পরে: [অধ্যায় ১৪: Logout, Revocation এবং Session Recovery](../14-logout-revocation/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
