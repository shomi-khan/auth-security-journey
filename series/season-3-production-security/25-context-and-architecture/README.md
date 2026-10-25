---
chapter: 24
season: 3
title: "Context বদলালে Architecture কেন বদলায়?"
status: planned
---

# Context বদলালে Architecture কেন বদলায়?

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
আগের chapter-গুলোতে আলাদা আলাদা security control শেখা হয়েছে। এই অধ্যায়ে দেখানো হবে, user, client, data sensitivity, operational capacity ও threat model বদলালে architecture decision-ও বদলায়। কোনো একটি pattern-কে সব পরিস্থিতির universal answer হিসেবে দেখানো হবে না।

## গল্পের শুরু
KenaKata এখন ছোট দোকান নয়। একাধিক organization, SPA, mobile app, Google login, connected application, sensitive refund এবং microservice আছে। আগের session-based design-এর কিছু অংশ এখনও কাজে লাগে, কিছু অংশ নতুন boundary-তে যথেষ্ট নয়।

## আলোচ্য বিষয়
- Requirement ও threat model থেকে architecture decision নেওয়া
- Browser session, BFF, access token ও refresh token-এর উপযুক্ত context
- Authentication বনাম authorization boundary
- Browser, identity provider, authorization server, gateway ও backend service-এর trust boundary
- Token audience, service identity এবং resource-level authorization
- Revocation, logging, monitoring, recovery ও incident response
- Usability, availability, latency, complexity ও operational maturity-এর trade-off
- Decision record: context, options, constraints, risks, chosen option ও revisit trigger
- Product timeline-এর সঙ্গে architecture evolution মেলানো

## অধ্যায় শেষে পাঠক পারবে
- Requirement থেকে security architecture-এর প্রশ্ন বের করতে
- দুইটি plausible design-এর trade-off তুলনা করতে
- Architecture decision ও তার assumption লিখে রাখতে
- Security control-এর সীমা ও residual risk স্পষ্ট করতে

## এই অধ্যায়ে থাকবে না
- একটিমাত্র “সেরা” architecture ঘোষণা
- Vendor-specific deployment blueprint
- নতুন authentication protocol শেখানো; এটি synthesis chapter

## ধারাবাহিকতা
আগে: [অধ্যায় ২৩: Machine-to-Machine Authentication](../23-machine-to-machine/README.md)
পরে: [অধ্যায় ২৫: Capstone](../25-capstone/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
