# Season 3 — Production Security

অধ্যায় ২২–২৫। একটাও এখনো লেখা হয়নি।

শেষ ছবি [architecture](../../product/architecture.md)-এ। সেটা শেষ অবস্থা। আগের অধ্যায় সেটাকে বর্তমান system ধরে নেবে না। কখন কী চালু হয়, সেটা [timeline](../../product/timeline.md)-এ।

## এই Season কেন পড়ব?

একটা দোকানের login দিয়ে পুরো SaaS চলে না। Refund-এর মতো কাজে আরেক ধাপ লাগে। Service যখন আরেক service-কে ডাকে, সেখানে মানুষের session কাজ করে না। আগের সব সিদ্ধান্ত এক architecture-তে বসাতে হয়।

Capstone-এ টুকরোগুলো একসঙ্গে পড়া হয়। Trade-off লুকিয়ে ফেলা হয় না।

## কী কী সমস্যা solve করব?

- সংবেদনশীল কাজে শুধু একবার login থাকা
- মানুষের credential আর service-এর credential গুলিয়ে ফেলা
- পুরনো context-এর architecture নতুন product-এ আটকে রাখা

## কোন concepts শিখব?

MFA, Step-up Authentication, Machine-to-Machine Authentication, architecture কেন বদলায়, আর পুরো production architecture।

## Chapter list

22. [MFA এবং Step-up Authentication](22-mfa-step-up/README.md)
23. [Machine-to-Machine Authentication](23-machine-to-machine/README.md)
24. [Context বদলালে Architecture কেন বদলায়?](24-context-and-architecture/README.md)
25. [Capstone: পুরো KenaKata Security Architecture](25-capstone/README.md)

## Season শেষে reader কী বুঝতে পারবে?

- কোন কাজে step-up লাগে, আর সাধারণ session আলাদা নিয়ন্ত্রণ
- মানুষের credential আর service-এর credential আলাদা
- শেষ architecture-এর সীমানা: client, gateway, service, authorization, identity provider, আর connected application
- প্রথম দোকানের password থেকে শুরু করে একটা কাজ এখন কোন নিয়ন্ত্রণের মধ্যে, সেটা ধরে ধরে বলা

[সিরিজের সূচি](../../README.md)
