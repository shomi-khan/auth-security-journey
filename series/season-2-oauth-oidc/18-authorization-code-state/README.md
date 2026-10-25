---
chapter: 18
season: 2
title: "Authorization Code Flow এবং state"
status: planned
---

# Authorization Code Flow এবং state

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
OAuth-এর actor-দের চেনার পর authorization request থেকে token পাওয়া পর্যন্ত Authorization Code Flow-এর sequence বুঝতে হবে. `state` কী ঝুঁকি কমায় এবং client-কে request-টির response-এর সঙ্গে কীভাবে যুক্ত রাখে, সেটাই মূল ফোকাস।

## গল্পের শুরু
রাফি হিসাবরক্ষার app-কে access দিতে সম্মতি দেয়। Browser authorization server থেকে ফিরে আসে, কিন্তু callback-এর response-টি কি সত্যিই আরিফের শুরু করা request-এর? নীরা এমন একটি flow চায় যেখানে login/consent-এর ফল অন্য browser action-এর সঙ্গে গুলিয়ে না যায়।

## আলোচ্য বিষয়
- Browser, client, authorization server ও resource server-এর sequence
- Authorization request, user consent, authorization code এবং back-channel token exchange
- Authorization code কেন access token নয় এবং কেন স্বল্পস্থায়ী/একবার ব্যবহারযোগ্য হওয়া দরকার
- `state` তৈরি, সংরক্ষণ, callback-এ মিলিয়ে দেখা এবং mismatch হলে fail closed
- Login CSRF ও request/response correlation-এর threat model
- Redirect-এর পর query parameter, browser history, log ও referrer-এর exposure
- Public ও confidential client-এর পার্থক্য, secret-এর সীমাবদ্ধতা
- Flow diagram-এ front-channel বনাম back-channel আলাদা করা

## অধ্যায় শেষে পাঠক পারবে
- Authorization Code Flow ধাপে ধাপে আঁকতে
- `state` কী রক্ষা করে এবং কী রক্ষা করে না তা বলতে
- Authorization code-কে token হিসেবে ভুল ব্যবহার না করতে
- Callback handler-এর জন্য গুরুত্বপূর্ণ validation checklist তৈরি করতে

## এই অধ্যায়ে থাকবে না
- Redirect URI-র নির্ভুল matching ও code interception-এর পূর্ণ বিশ্লেষণ; অধ্যায় ১৮
- PKCE-এর বিস্তারিত; অধ্যায় ১৯
- User identity-এর OIDC claims; অধ্যায় ২০

## ধারাবাহিকতা
আগে: [অধ্যায় ১৬: OAuth 2.0 কেন?](../17-oauth/README.md)
পরে: [অধ্যায় ১৮: Redirect URI এবং Authorization Code Theft](../19-redirect-uri-code-theft/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
