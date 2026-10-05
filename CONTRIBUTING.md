# Contributing

অধ্যায় একটা একটা করে লেখা হবে। প্রতিটা অধ্যায়ের ডিরেক্টরি আগে থেকেই আছে। ওই অধ্যায়ের `README.md` সম্পাদনা করবে। নতুন ফোল্ডার খুলবে না, আর একসাথে কয়েকটা অধ্যায় লিখে ফেলবে না।

> নতুন chapter লেখার সময় KenaKata-এর established product universe ভাঙা যাবে না।

লেখার আগে [`product/`](product/README.md) পড়ে নাও। দৃশ্যে এমন কোনো feature, চরিত্র, বা architecture লাগলে যেটা ওই ডকুমেন্টে এখনো নেই, দৃশ্য বদলাও। চুপিচুপি KenaKata বদলাবে না। নতুন তথ্য সত্যি দরকার হলে আগে product ফাইল বদলাও, তারপর [`notes/decision-log.md`](notes/decision-log.md)-এ কারণ লেখো।

## কাজের ধাপ

```text
১. পরবর্তী chapter নির্বাচন
        ↓
২. product/ documentation পড়া
        ↓
৩. আগের chapter পড়া
        ↓
৪. Problem এবং narrative ঠিক করা
        ↓
৫. Chapter লেখা
        ↓
৬. Technical accuracy check
        ↓
৭. Security accuracy check
        ↓
৮. Diagram / Code যোগ করা
        ↓
৯. Chapter status update
        ↓
১০. Season এবং Root README update
```

১. **পরবর্তী chapter নির্বাচন।** যে অধ্যায় এখনো প্রকাশিত হয়নি, পরেরটা লেখো। তালিকায় অন্য কথা না থাকলে ক্রম মানো।
২. **`product/` পড়া।** [product.md](product/product.md), [characters.md](product/characters.md), [domain-model.md](product/domain-model.md), [timeline.md](product/timeline.md), [architecture.md](product/architecture.md)। Feature আগে থেকে আছে কি না, সেটা timeline বলে।
৩. **আগের chapter পড়া।** নতুন অধ্যায় আগের সমস্যার পরের ধাপ। Product আবার শূন্য থেকে শুরু হয় না।
৪. **Problem আর narrative ঠিক করা।** নিচের ছক অনুসরণ করো। কোন threat, কোন সীমা, কোন trade-off — এগুলো না ভেবে লেখা শুরু করবে না।
৫. **Chapter লেখা।** ওই অধ্যায়ের `README.md`-এ placeholder সরাও। ভাষা বাংলা। Technical term ইংরেজিতে থাকবে।
৬. **Technical accuracy।** নাম, প্রবাহ, আর “এটা কী করে” — [`references/`](references/README.md)-এর সোর্স খুলে মিলাও। ব্লগের কথাকে spec-এর উপরে বসাবে না।
৭. **Security accuracy।** একই সোর্স। “JWT secure” এই ধরনের কথা লিখবে না, যদি না সঙ্গে থাকে কোন threat, কোন property, কোথায় সীমা, কী trade-off। নতুন ভুল ধারণা ধরা পড়লে ছোট করে [misconceptions](notes/misconceptions.md)-এ যোগ করো। ব্যাখ্যা অধ্যায়েই থাকবে।
৮. **Diagram বা code।** লাগলে যোগ করো। না লাগলে যোগ করবে না। ছোট ASCII ছবি অধ্যায়ের ভিতরেই ভালো। বারবার লাগলে [diagrams/](diagrams/authentication/)-এ রাখো। Go উদাহরণ [examples/go/](examples/go/authentication/)-এ: ছোট, একটা ধারণা, একা পড়লে বোঝা যায়। KenaKata-র পুরো backend এখানে বানাবে না।
৯. **Chapter status।** নিচের বাক্সে টিক তখনই, যখন সেই ধাপ সত্যি শেষ। `Published`-এ টিক মানে অধ্যায় প্রকাশিত।
১০. **Season README আর root README।** অধ্যায় প্রকাশিত না হওয়া পর্যন্ত সূচিপত্রে `[ ] লেখা হয়নি` থাকবে। প্রকাশিত হলে `[x]` করো, আর “লেখা হয়নি” সরিয়ে দাও।

## গল্প ভাঙবে না

- চরিত্র তিনজন: আরিফ, নীরা, রাফি। নতুন নিয়মিত চরিত্র লাগলে আগে decision log।
- ক্রেতা customer record। এই সিরিজে ক্রেতা login করে না।
- Role চারটা: Owner, Admin, Member, Viewer। Role থাকে membership-এ। Team আলাদা role দেয় না।
- Google login, বাইরের application, MFA, আর microservice — timeline যে ধাপে আনে, তার আগে product-এ নেই।
- [architecture.md](product/architecture.md) শেষ অবস্থা। শুরুর অধ্যায় একটাই web application।

## ভাষা

পাঠক junior engineer। বাংলা স্বাভাবিক হবে, অনুবাদের মতো নয়।

ভালো:

> Server-কে আগে নিশ্চিত হতে হবে user আসলে কে। এই identity verify করার কাজটাই Authentication।

এড়িয়ে চলো:

> Authentication mechanism দ্বারা user identity validation সম্পাদিত হয়।

Technical term-এর জোর করা বাংলা বসাবে না। “প্রমাণীকরণ”, “অনুমোদন”, “পরিচয়-সনাক্তকরণ” লিখবে না। প্রথমবার দরকার হলে সহজ বাংলায় বলে দাও, তারপর ইংরেজি নামই চালাও।

> **Authentication** হলো “তুমি কে?” সেটা verify করার প্রক্রিয়া।

বাক্য ছোট রাখো। যেখানে পারো KenaKata-র উদাহরণ দাও। অপ্রয়োজনীয় jargon জড়াবে না। আবার পাঠককে নতুন কেউ ভাবে নিয়ে ব্যাখ্যাও করবে না। সহজ ভাষা মানে বিষয়টা হালকা করে দেওয়া নয়।

`product/`, `references/`, `notes/`, season README — এগুলোও বাংলা। Spec-এর অফিসিয়াল নাম ইংরেজিতে থাকবে। [LICENSE](LICENSE) ইংরেজিতেই থাকে।

## একটা বড় বিষয় এলে

অধ্যায় যখন কোনো বড় জিনিস শেখায়, এই প্রশ্নগুলো ছুঁয়ে যাও:

- এটা কী
- কেন এসেছে
- কোন সমস্যা সমাধান করে
- কোন সমস্যা সমাধান করে না
- কোথায় থাকে
- কীভাবে আক্রমণ হতে পারে
- compromised হলে ক্ষতি কী
- lifetime
- revocation
- বিকল্প কী
- production-এ কী ছাড় দিতে হয়

এখনই এই প্রশ্নগুলোর উত্তর এই ফাইলে লিখে রেখো না। উত্তর অধ্যায়ে, সোর্স দেখে।

## যা এখানে আসবে না

Site generator, অ্যাপ, package manifest, Go module, বা CI pipeline যোগ করবে না। এই repository Markdown। কোড তখনই, যখন একটা অধ্যায়ের একটা ধারণা বোঝাতে ছোট উদাহরণ লাগে।
