---
chapter: 21
season: 2
title: "Google Login এবং Token Validation"
status: planned
---

# Google Login এবং Token Validation

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
OIDC-এর ধারণাকে KenaKata-র Google login feature-এ প্রয়োগ করা। Browser callback থেকে পাওয়া তথ্য দিয়ে account তৈরি বা login করার আগে কী validate করতে হবে, এবং external identity-কে local account-এর সঙ্গে কীভাবে নিরাপদে যুক্ত করতে হয়, তা বোঝানো হবে।

## গল্পের শুরু
রাফি চায় ক্রেতা বা কর্মীরা Google দিয়ে login করুক। আরিফ callback-এ পাওয়া email-কে user identity ধরে নেয়। নীরা জিজ্ঞেস করে: email claim যাচাই না করলে, issuer বা audience না মিললে, অথবা একই email পরে অন্য identity-তে গেলে কী হবে?

## আলোচ্য বিষয়
- OIDC provider হিসেবে Google এবং relying party হিসেবে KenaKata
- Authorization Code Flow ও PKCE-র সঙ্গে Google login-এর সম্পর্ক
- ID Token validation: signature, issuer, audience, expiry এবং প্রাসঙ্গিক nonce
- Email claim, email verification এবং stable subject identifier-এর পার্থক্য
- External identity-কে local account-এর সঙ্গে link করার নিরাপদ নীতি
- Account linking, duplicate account এবং identity collision-এর ঝুঁকি
- Login callback-এর error handling, session creation এবং audit event
- ID Token বনাম Access Token: কোনটি কোন উদ্দেশ্যে
- Google-এর token-কে KenaKata-র API-তে স্বয়ংক্রিয়ভাবে গ্রহণ না করা
- Timeline: এই অধ্যায়েই Google login feature চালু হয়

## অধ্যায় শেষে পাঠক পারবে
- Google login-এ OIDC-এর অংশগুলো চিহ্নিত করতে
- Token validation-এর গুরুত্বপূর্ণ check ব্যাখ্যা করতে
- Email আর stable external subject-এর পার্থক্য বুঝতে
- Account linking-এর জন্য নিরাপদ policy-র প্রশ্নগুলো তৈরি করতে

## এই অধ্যায়ে থাকবে না
- OAuth client হিসেবে অন্য application-কে KenaKata access দেওয়া; সেটি Season 2-এর পরের product milestone
- Google-এর সব API scope-এর আলোচনা
- Framework-specific setup tutorial

## ধারাবাহিকতা
আগে: [অধ্যায় ২০: OIDC](../20-oidc/README.md)
পরে: [অধ্যায় ২২: MFA এবং Step-up Authentication](../../season-3-production-security/22-mfa-step-up/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
