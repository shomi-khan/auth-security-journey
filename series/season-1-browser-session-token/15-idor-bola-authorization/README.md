---
chapter: 15
season: 1
title: "IDOR/BOLA এবং Authorization Design"
status: planned
---

# IDOR/BOLA এবং Authorization Design

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
Authentication ঠিক থাকলেও একজন user অন্যের order দেখতে বা বদলাতে পারে, যদি server resource ownership ও permission যাচাই না করে। এই অধ্যায় object-level authorization-এর mental model তৈরি করবে এবং KenaKata-র এক দোকান থেকে multi-organization SaaS-এ যাওয়ার সময় authorization design কীভাবে বদলায় তা দেখাবে।

## গল্পের শুরু
রাফি order-এর URL-এ 1051 বদলে 1052 করে অন্য customer-এর order দেখতে পায়। দুজন user-ই সঠিকভাবে login করেছে। সমস্যা পরিচয় যাচাইয়ে নয়, নির্দিষ্ট resource-এর ওপর অনুমতিতে।

## আলোচ্য বিষয়
- IDOR ও BOLA: একই ধরনের object-level authorization failure-কে ভিন্ন নামে দেখা
- URL বা request body-তে ID থাকা কেন permission নয়
- Function-level permission বনাম object-level permission
- Subject, action, resource ও context দিয়ে authorization প্রশ্ন গঠন
- Tenant/organization boundary, membership এবং role
- Server-side enforcement: list, read, update, delete, export এবং nested resource
- Query scoping ও resource lookup-এর design trade-off
- Deny-by-default, least privilege এবং প্রতিটি request-এ authorization
- 401, 403, 404-এর অর্থ ও তথ্য ফাঁসের trade-off
- Security test: user A দিয়ে user B-এর resource access করার negative cases
- অধ্যায় ২-এর authorization ধারণা এবং product timeline-এর organization model-এর সঙ্গে সংযোগ

## অধ্যায় শেষে পাঠক পারবে
- Authentication bug আর authorization bug আলাদা করতে
- একটি API endpoint-এর object-level authorization check লিখে ব্যাখ্যা করতে
- Multi-tenant data isolation-এর threat model তৈরি করতে
- Authorization test-এ cross-user ও cross-organization case অন্তর্ভুক্ত করতে

## এই অধ্যায়ে থাকবে না
- OAuth delegation flow; অধ্যায় ১৬–২০
- পূর্ণ RBAC/ABAC policy engine implementation
- নির্দিষ্ট database বা framework-এর tutorial

## ধারাবাহিকতা
আগে: [অধ্যায় ১৪: Logout, Revocation এবং Session Recovery](../14-logout-revocation/README.md)

পরে: [অধ্যায় ১৬: OAuth 2.0 কেন?](../../season-2-oauth-oidc/16-oauth/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
