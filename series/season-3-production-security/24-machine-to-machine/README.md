---
chapter: 24
season: 3
title: "Machine-to-Machine Authentication"
status: planned
---

# Machine-to-Machine Authentication

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
KenaKata-র backend একাধিক service-এ ভাগ হলে service-to-service request-এ মানুষের browser session বা user password ব্যবহার করা ঠিক হবে না। Workload identity, service credential এবং least-privilege access-এর mental model তৈরি হবে।

## গল্পের শুরু
Order service invoice তৈরি করতে billing service-কে ডাকে। আরিফ দ্রুত সমাধান হিসেবে একটি স্থায়ী shared API key সব service-এ বসিয়ে দেয়। নীরা জানতে চায়, একটি service compromise হলে কতগুলো service-এর access হারাবে এবং credential rotate কীভাবে হবে।

## আলোচ্য বিষয়
- Human identity বনাম workload/service identity
- Shared static secret-এর blast radius ও rotation cost
- Service credential, short-lived credential এবং least privilege
- Service-to-service authentication ও authorization আলাদা করা
- Audience, scope/permission এবং service-specific access
- API gateway থাকলেও downstream service কেন নিজস্ব trust boundary রাখে
- mTLS, workload identity ও signed token-এর high-level trade-off
- Secret distribution, rotation, revocation এবং audit logging
- User context service call-এ বহন করলে delegation/confused-deputy risk
- Failure mode: credential leak, wrong audience, overly broad privilege

## অধ্যায় শেষে পাঠক পারবে
- Service identity ও user identity আলাদা করতে
- একটি service-to-service trust boundary আঁকতে
- Shared secret-এর বিকল্প ও trade-off তুলনা করতে
- Service credential-এর lifecycle ও minimum privilege নির্ধারণের প্রশ্ন করতে

## এই অধ্যায়ে থাকবে না
- Kubernetes বা cloud provider-এর নির্দিষ্ট setup
- পূর্ণ PKI বা certificate operations tutorial
- End-to-end architecture capstone; অধ্যায় ২৬

## ধারাবাহিকতা
আগে: [অধ্যায় ২৩: MFA এবং Step-up Authentication](../23-mfa-step-up/README.md)
পরে: [অধ্যায় ২৫: Context বদলালে Architecture কেন বদলায়?](../25-context-and-architecture/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
