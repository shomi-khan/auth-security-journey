# KenaKata

এই ফাইল chapter নয়। লেখার সময় মিলিয়ে নেওয়ার জন্য।

## KenaKata কী?

KenaKata একটা fictional SaaS commerce platform। নামটা কেনাকাটা থেকে।

ছোট ব্যবসা এখানে নিজেদের দোকান চালায়:

- Product manage করে
- Order manage করে
- Customer manage করে
- Team member যোগ করে
- Organization manage করে
- External application connect করে

শুরুতে এটা একটা সাধারণ e-commerce application। তারপর ধীরে ধীরে multi-tenant SaaS platform হয়। শেষে web application, একটা SPA, আর একটা mobile application থাকে।

যারা login করে তারা দোকানের মালিক আর কর্মী। ক্রেতা মানে customer record। এই সিরিজে ক্রেতার login নেই।

আরিফ এটা বানায়। নীরা ব্যবসার দাবি আনে। রাফি প্রথম দোকানদার। চরিত্রের বিস্তারিত [characters.md](characters.md)-এ।

## শুরুর product

একটা দোকান, একটা server-rendered web application। Product, customer, আর order আছে।

এখনো নেই: organization, role, mobile app, SPA, Google login, বাইরের application, MFA, আলাদা আলাদা service। এগুলো কখন আসে, সেটা [timeline.md](timeline.md)-এ।

রাফির পোশাকের দোকানটাই এই প্রথম দোকান। আলাদা ব্র্যান্ডের নাম নেই।

## কাদের জন্য

- ছোট ব্যবসার মালিক, যে নিজে সফটওয়্যার বানায় না। রাফি সেই মানুষ।
- দোকানের কর্মী, যারা product আর order সামলায়।
- পরে আরও ব্যবসা। প্রত্যেকটা আলাদা organization। অন্যের data দেখে না।

## ব্যবসায়িক মডেল

প্রথমে এক দোকানের অ্যাপ। পরে organization-কে subscription হিসেবে বিক্রি হয়। Billing product-এর অংশ, আর সেটা সংবেদনশীল কাজ।

দাম, প্ল্যানের নাম, বা মুদ্রা এখানে ধরা নেই। কোনো অধ্যায়ে অঙ্ক লাগলে সেটা দৃশ্যের অংশ। নতুন দামের তালিকা বানিয়ে ফেলবে না।

## যা যা করতে পারে

নিচের কাজগুলো পুরো সিরিজ জুড়ে আসে। কোনটা কোন ধাপে, সেটা timeline বলে।

- Product manage করা
- Customer manage করা
- Order manage করা
- Team বানানো
- Organization-এর member সামলানো
- Billing-এর কাজ
- External application connect করা
- Web আর mobile application ব্যবহার করা

## কীভাবে বদলায়

এক দোকান থেকে শুরু। তারপর email আর password, তারপর cookie-তে session। Mobile এলে access token আর refresh token। তারপর SPA। তারপর এক দোকান একটা organization হয়, role আসে। তারপর Google login, তারপর বাইরের application। সংবেদনশীল কাজে MFA। শেষে backend কয়েকটা service-এ ভাগ হয়।

ধাপের বিস্তারিত [timeline.md](timeline.md)। শেষ চেহারা [architecture.md](architecture.md)।

## Web application

প্রথম interface। Server-rendered website। Login থাকলে browser cookie-তে session থাকে। শুরুর অধ্যায় এই অ্যাপ নিয়ে।

SPA আসার পরও এই website থাকতে পারে। সিরিজ এটাকে মুছে ফেলতে বলে না।

## SPA

পরে আসা আরেকটা browser client, একই ব্যবসার জন্য। প্রথম website-এর থেকে আলাদা। Token browser-এ কোথায় থাকবে, সেটা আলাদা অধ্যায়ের বিষয়। এখানে সমাধান লেখা নেই।

## Mobile application

মালিক আর কর্মীদের ফোন। ক্রেতার অ্যাপ নয়। Browser cookie আর একমাত্র রাস্তা না হলে token-এর কথা ওঠে।

## Organization

একটা ছোট ব্যবসা। তার product, customer, order, member, team, billing, আর connected application এই organization-এর।

এর আগে গল্পে একটাই দোকান। পরে রাফির দোকান একটা organization। অন্য ব্যবসা তার data দেখে না। একজন মানুষ একাধিক organization-এর member হতে পারে।

## Team

এক organization-এর ভিতরে মানুষের দল। যেমন কারা order সামলায়। Team আলাদা role দেয় না। কে কী করতে পারবে, সেটা membership-এর role। Role-এর ছক [domain-model.md](domain-model.md)-এ।

## বাইরের application

Organization একটা external application connect করতে পারে, যাতে ওই app তার data নিয়ে কাজ করতে পারে। ওই app একটা OAuth client। এই সম্পর্কে KenaKata authorization server।

Google login উল্টো দিক। সেখানে Google identity provider, KenaKata client।

## যে কাজগুলো সংবেদনশীল

MFA-র ধাপ আসার পর শুধু login থাকা যথেষ্ট ধরা হয় না। তার আগের অধ্যায়ে এগুলো সাধারণ request হতে পারে।

- Billing বা payment method বদলানো
- Member যোগ করা, বাদ দেওয়া, বা role বদলানো
- Ownership হস্তান্তর
- External application connect বা disconnect করা
- Organization মুছে ফেলা, বা data বের করে নেওয়া
- Refund

কোন উপায়ে দ্বিতীয় ধাপ হবে — TOTP, passkey, বা অন্য কিছু — এখানে ধরা নেই। অধ্যায় একটা উপায় বেছে নিলে বলবে, এটা ওই দৃশ্যের পছন্দ।

## Admin

Admin organization-এর একটা role। আলাদা KenaKata-স্টাফের পণ্য নয়।

Owner organization ধরে রাখে। Admin দৈনন্দিন কাজ চালায়: member, connected app, refund। Ownership হস্তান্তর Owner-এর কাজ।

আলাদা platform-admin চরিত্র নেই। দরকার হলে আগে decision log-এ লিখতে হবে।

## শেষ অবস্থা

সিরিজের শেষে KenaKata একটা multi-tenant SaaS:

- মালিক আর কর্মীদের জন্য web, SPA, আর mobile
- Organization, membership, role, আর team
- Product, customer, আর order প্রতিটা organization-এ আলাদা
- Google একটা external identity provider
- বাইরের application OAuth দিয়ে connected
- সংবেদনশীল কাজে MFA আর step-up
- API gateway-এর পিছনে user, order, billing, আর notification service
- Protected কাজের আগে একটা authorization layer

এই ছবি [architecture.md](architecture.md)-এ। অধ্যায় ২৪-এর আগে পুরো ছবিটাকে বর্তমান system ধরে নেওয়া যাবে না।
