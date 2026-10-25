---
chapter: 20
season: 2
title: "OIDC: OAuth-এর ওপর Identity Layer"
status: planned
---

# OIDC: OAuth-এর ওপর Identity Layer

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
OAuth access delegation ব্যাখ্যা করে, কিন্তু একা user identity প্রমাণ করার পূর্ণ protocol নয়। OpenID Connect (OIDC) কীভাবে OAuth flow-এর ওপর identity layer যোগ করে, ID Token কী তথ্য বহন করে, এবং client কীভাবে তা validate করে, তা শেখানো হবে।

## গল্পের শুরু
হিসাবরক্ষার app KenaKata-র API-তে access token দিয়ে report আনতে পারে। আরিফ ধরে নেয় token পেলেই user-এর পরিচয় জানা গেছে। নীরা প্রশ্ন করে: access token-টি API-র জন্য, নাকি client-কে user-এর login সম্পর্কে কিছু বলার জন্য?

## আলোচ্য বিষয়
- OAuth ও OIDC-এর উদ্দেশ্যের পার্থক্য
- OpenID scope এবং ID Token-এর ভূমিকা
- ID Token বনাম Access Token: recipient, purpose ও validation
- Issuer, audience, signature, expiry এবং nonce-এর ধারণা
- ID Token-এর claims-কে অন্ধভাবে বিশ্বাস না করা
- User identity, subject identifier এবং account linking-এর উচ্চস্তরের ঝুঁকি
- Authentication event/claim ও application session-এর সম্পর্ক
- Discovery ও JWKS-এর ধারণা, key rotation-এর প্রাথমিক প্রভাব
- ID Token-কে resource API-র access token হিসেবে ব্যবহার না করা

## অধ্যায় শেষে পাঠক পারবে
- OAuth ও OIDC আলাদা করে ব্যাখ্যা করতে
- ID Token-এর গুরুত্বপূর্ণ validation requirement চিহ্নিত করতে
- ID Token ও Access Token-এর ভুল বিনিময় শনাক্ত করতে
- Identity provider-এর subject-কে application account-এর সঙ্গে যুক্ত করার ঝুঁকি বুঝতে

## এই অধ্যায়ে থাকবে না
- Google-এর console/setup ধাপে ধাপে; অধ্যায় ২১
- OAuth flow-এর পুনরাবৃত্ত পূর্ণ walkthrough
- নির্দিষ্ট OIDC library-র implementation

## ধারাবাহিকতা
আগে: [অধ্যায় ১৯: PKCE](../19-pkce/README.md)
পরে: [অধ্যায় ২১: Google Login এবং Token Validation](../21-google-login/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
