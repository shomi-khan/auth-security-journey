# Domain model

এটা blog series-এর conceptual model। এটা production database schema নয়।

অধ্যায় কোনো threat বোঝাতে একটা ছক আঁকতে পারে। সেই ছককে এখানকার canonical schema বানিয়ে ফেলবে না। নিচের নামগুলো ধারণা। Field শুধু সেইটুকু, যেটুকু না থাকলে অধ্যায়গুলো পরস্পরের সঙ্গে মিল হারায়।

## ধারণাগুলো

| নাম | কী |
| --- | --- |
| User | যে login করতে পারে। মালিক বা কর্মী |
| Customer | দোকানের ক্রেতার রেকর্ড। ক্রেতা login করে না |
| Organization | প্ল্যাটফর্মের একটা ছোট ব্যবসা। তার data-র সীমানা |
| Membership | একজন user আর একটা organization-এর যোগ। এখানে role থাকে |
| Team | এক organization-এর ভিতরে মানুষের দল। Team role দেয় না |
| Product | organization যা বিক্রি করে |
| Order | ওই organization-এর একটা বিক্রি |
| Session | Cookie-র সময়ে server যে login-এর রেকর্ড রাখে |
| OAuthClient | বাইরের সেই application, যে organization-এর resource নিয়ে কাজ করতে পারে |
| ExternalIdentity | একজন user-এর সঙ্গে বাইরের identity provider-এর অ্যাকাউন্টের যোগ |

Customer আর Team এই তালিকায় আছে, কারণ দোকান ক্রেতা রাখে আর পরে দল বানায়। বাকি নামগুলো সিরিজের মূল মডেল।

`Session`, `OAuthClient`, আর `ExternalIdentity` পরে দরকার হবে বলে নামগুলো আগে থেকেই ধরা। শুরুতে এগুলো বানানো নেই। কখন আসে, সেটা [timeline.md](timeline.md)।

## সম্পর্ক

```text
User
  |
  +---- Membership ---- Organization
  |                         |
  |                         +---- Team
  |                         +---- Product
  |                         +---- Customer
  |                         +---- Order
  |                         +---- OAuthClient
  |
  +---- Session
  |
  +---- ExternalIdentity
```

একজন user একাধিক organization-এর member হতে পারে। একটা membership একজন user আর একটা organization-এর।

Product, customer, order, team, আর connected client একটা organization-এর। Organization আসার আগে এই ব্যবসার রেকর্ড একটাই দোকানের। পরে সেই দোকান রাফির organization হয়।

Team-এ এমন user থাকে, যার ওই organization-এ membership আগে থেকেই আছে।

Session একজন user-এর। External identity-ও একজন user-এর। OAuth client একটা organization-এর প্রসঙ্গে connected।

Order কোন product আর কোন customer-কে ধরে, সেটা সাধারণ দোকানের তথ্য। সেটা নিজে থেকে authorization-এর নিয়ম নয়।

## Role

Role থাকে `Membership`-এ। Team-এ নয়। Token-এর ভিতরের একটা লেখাও আলাদা role-ব্যবস্থা নয়।

```text
Owner
Admin
Member
Viewer
```

এই সরল মডেলে একটা organization-এ একজন Owner। Ownership হস্তান্তর করা যায়। সেই হস্তান্তর সংবেদনশীল কাজ।

| Role | কী করতে পারে |
| --- | --- |
| Owner | Admin যা পারে তার সব, আর ownership হস্তান্তর, আর organization রাখা বা মুছে ফেলা |
| Admin | দৈনন্দিন ব্যবস্থাপনা: member, Owner বাদে role বদল, setting, connected application, refund |
| Member | Product, customer, আর order তৈরি ও বদল |
| Viewer | Organization-এর তথ্য শুধু দেখে |

এই ছক যাতে অধ্যায়গুলো পরস্পরের উল্টো না বলে। কোনো অধ্যায় একটা সূক্ষ্ম নিয়ম দেখাতে পারে, যেমন “member দিতে পারে শুধু Admin”। নতুন role বানাবে না। ক্ষমতার ক্রম Owner, তারপর Admin, তারপর Member, তারপর Viewer।

Token-এ role লেখা থাকলে সেটা membership-এর ইঙ্গিত হতে পারে। সেই লেখাই শেষ সিদ্ধান্ত নয়। Resource যেন ওই membership-এর organization-এর হয়, সেটা আলাদা করে দেখতে হয়। এই পার্থক্য অধ্যায় ১৫-এর বিষয়। এখানে নিয়মটা শুধু মিল রাখার জন্য।
