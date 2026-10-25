---
chapter: 19
season: 2
title: "Redirect URI এবং Authorization Code Theft"
status: planned
---

# Redirect URI এবং Authorization Code Theft

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
Authorization Code Flow-এ browser-কে কোথায় ফেরানো হবে, সেটি শুধু navigation detail নয়। Redirect URI validation-এর ভুল authorization code ভুল জায়গায় পাঠাতে পারে। এই অধ্যায় redirect matching, code interception এবং code-এর ক্ষতি সীমিত করার নকশা বোঝাবে।

## গল্পের শুরু
নীরা দেখে integration-এর redirect URL-এ সামান্য পরিবর্তন করেও authorization server code ফেরত দিচ্ছে। আরিফ বলে domain তো একই। কিন্তু path, query, wildcard ও open redirect-এর ফাঁক দিয়ে code অন্য জায়গায় চলে যেতে পারে।

## আলোচ্য বিষয়
- Redirect URI-র allowlist ও exact matching-এর উদ্দেশ্য
- Wildcard, prefix matching, untrusted query parameter এবং open redirect-এর ঝুঁকি
- Authorization code-এর স্বল্প lifetime ও একবার ব্যবহারযোগ্যতা
- Browser/front-channel-এ code প্রকাশ পাওয়ার সম্ভাব্য পথ
- Code interception এবং code injection-এর উচ্চস্তরের threat model
- Redirect URI validation ও `state`-এর আলাদা দায়িত্ব
- PKCE কীভাবে intercepted code-এর ব্যবহার কঠিন করে, পরের অধ্যায়ের preview
- Production callback URL পরিবর্তনের operational trade-off

## অধ্যায় শেষে পাঠক পারবে
- দুর্বল redirect matching-এর উদাহরণ শনাক্ত করতে
- Redirect URI, `state` এবং PKCE-এর কাজ গুলিয়ে না ফেলতে
- Authorization code কোথায় leak হতে পারে তা threat model-এ চিহ্নিত করতে
- Redirect allowlist review-এর checklist তৈরি করতে

## এই অধ্যায়ে থাকবে না
- PKCE protocol detail; অধ্যায় ১৯
- OIDC ID Token validation; অধ্যায় ২০
- Provider-specific console configuration walkthrough

## ধারাবাহিকতা
আগে: [অধ্যায় ১৭: Authorization Code Flow এবং state](../18-authorization-code-state/README.md)
পরে: [অধ্যায় ১৯: PKCE](../20-pkce/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
