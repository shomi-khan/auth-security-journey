---
chapter: 26
season: 3
title: "Capstone: পুরো KenaKata Security Architecture"
status: planned
---

# Capstone: পুরো KenaKata Security Architecture

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
সিরিজের সব concept একত্র করে KenaKata-র production-oriented security architecture তৈরি ও review করা। এটি নতুন protocol-এর অধ্যায় নয়। পাঠককে আগের সিদ্ধান্তগুলো যুক্ত করে reasoning দেখাতে হবে, এবং কোথায় ঝুঁকি পুরোপুরি দূর হয়নি তা বলতে হবে।

## গল্পের শুরু
KenaKata-র নতুন organization customer, mobile app, SPA, Google login, third-party integration, sensitive refund এবং microservice ব্যবহার করছে। নীরা architecture review ডাকে: প্রতিটি request-এর identity, credential, permission এবং failure path কোথায় যাচাই হয়?

## আলোচ্য বিষয়
- System context ও component diagram
- Browser session/BFF, mobile token lifecycle এবং Google OIDC login-এর পৃথক flow
- OAuth delegated integration-এর scope ও consent boundary
- Access/refresh token expiry, rotation, revocation ও recovery
- Organization/resource-level authorization এবং IDOR/BOLA negative tests
- Sensitive action-এ MFA/step-up policy
- Service-to-service identity ও least privilege
- Logging, alerting, incident response এবং credential compromise recovery
- Threat model: attacker capability, assets, trust boundary, abuse case
- Architecture decision record, residual risk ও future review triggers

## Capstone deliverable
- একটি system context diagram এবং অন্তত তিনটি sequence diagram
- Credential lifecycle ও authorization boundary-এর তালিকা
- প্রধান abuse case ও control mapping
- Trade-off/decision log
- Residual risk এবং কোন assumption বদলালে architecture পুনর্বিবেচনা করতে হবে তার তালিকা

## অধ্যায় শেষে পাঠক পারবে
- Authentication, authorization ও delegated identity-র পূর্ণ flow trace করতে
- Architecture-কে threat model-এর সঙ্গে যুক্ত করতে
- Security control-এর সীমা ও residual risk যুক্তি দিয়ে ব্যাখ্যা করতে
- নতুন system design review-এ ব্যবহারযোগ্য প্রশ্ন করতে

## এই অধ্যায়ে থাকবে না
- Production certification বা formal penetration test-এর দাবি
- সব environment-এর জন্য copy-paste deployment recipe
- আগের chapter-এর ধারণা নতুন করে পূর্ণাঙ্গভাবে ব্যাখ্যা

## ধারাবাহিকতা
আগে: [অধ্যায় ২৫: Context বদলালে Architecture কেন বদলায়?](../25-context-and-architecture/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
