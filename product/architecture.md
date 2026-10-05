# Architecture

এটা **শেষ অবস্থার architecture**। সিরিজের শেষে KenaKata দেখতে এমন।

Service-গুলো ভাগ হয় অধ্যায় ২৩-এ, microservice-এর ধাপে। অধ্যায় ২৪ আর ২৫-এ এই ছবিটাই বর্তমান system। তার আগের অধ্যায় এই ছবিকে “এখন যা চলছে” ধরে নেবে না। সামনে কী আসবে সেটা বলতে চাইলে স্পষ্ট করে বলবে, এটা এখনো আসেনি।

ছবিটা conceptual। Implementation, deployment, বা header দিয়ে request-এর পথ এখানে নেই।

## শেষ ছবি

```text
Web / SPA
Mobile App
    |
    v
API Gateway
    |
    +---- User Service
    +---- Order Service
    +---- Billing Service
    +---- Notification Service
    |
    v
Authorization Layer
```

Client API gateway-এর সঙ্গে কথা বলে। Gateway সেই service-এ পাঠায়, যে ডেটার মালিক। Authorization layer বলে এই protected কাজ চলবে কি না। এই ছবিতে সেটা ওই কাজের পথে। কোনো অধ্যায়কে এটাকে নির্দিষ্ট লাইব্রেরি বা আলাদা সার্ভিস বানাতে হবে না।

Product catalog-এর আলাদা service নেই। ক্যাটালগের ডেটা commerce-এর দিকেই থাকে।

| অংশ | শেষ system-এ কী করে |
| --- | --- |
| Web / SPA | মালিক আর কর্মীদের browser client। পুরনো server-rendered অ্যাপ আর পরের SPA দুটোই থাকতে পারে |
| Mobile App | মালিক আর কর্মীদের client। Token ব্যবহার করে |
| API Gateway | Backend ভাগ হওয়ার পর client-এর সামনের দরজা |
| User Service | অ্যাকাউন্ট, login, session, আর external identity |
| Order Service | একটা organization-এর order |
| Billing Service | Billing-এর কাজ |
| Notification Service | Product যে বার্তা পাঠায়, যেমন মেইল |
| Authorization Layer | এই identity এই resource-এ এই কাজ করতে পারবে কি না |

## Google

Google একটা identity provider। KenaKata relying party। Google login সফল হলে একজন `User` মেলে বা নতুন হয়, আর একটা `ExternalIdentity` থাকে। Order Google-এর কাছে থাকে না। Google অ্যাকাউন্ট নিজে থেকে organization-এর role নয়।

## OAuth আর বাইরের application

Organization বাইরের একটা application connect করতে পারে। সেই application একটা `OAuthClient`। সে KenaKata-কে ডাকে organization-এর resource-এর জন্য। এই ডাকে KenaKata authorization server। দিকটা Google login-এর উল্টো।

## Service-to-service

Microservice-এ ভাগ হওয়ার পর service একে অপরকে ডাকে। ওই ডাকে ডাকা service-এর পরিচয় থাকে। মানুষের browser session বা মানুষের refresh token সেখানে ব্যবহার হয় না। এই বিষয় অধ্যায় ২৩। কীভাবে সেই পরিচয় প্রমাণ হয়, সেই বিস্তারিত এখানে নেই।

## MFA

[product.md](product.md)-এ যে কাজগুলো সংবেদনশীল, সেখানে session বা access token থাকলেও step-up লাগে। MFA সেই শেষ নিয়ন্ত্রণের অংশ। ছবিতে এটাকে ষষ্ঠ business service বানানো হয়নি।

## যা এই ছবিতে নেই

Message bus, service mesh, ক্লাউড অ্যাকাউন্ট, বা platform-admin-এর কনসোল এই সিরিজের architecture-এ নেই। Decision log-এ না নিলে অধ্যায়ে যোগ করবে না।
