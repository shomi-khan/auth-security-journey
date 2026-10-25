# Contributing

অধ্যায় একটা একটা করে লেখা হবে। সাধারণত প্রতিটা অধ্যায়ের ডিরেক্টরি আগে থেকেই আছে, তাই ওই অধ্যায়ের `README.md` সম্পাদনা করবে। পরিকল্পনায় অনুমোদিত নতুন অধ্যায় যোগ হলে নতুন directory তৈরি করা যাবে; একই পরিবর্তনে chapter numbering, folder slug, সব cross-reference, season index, root index এবং product timeline সামঞ্জস্য করতে হবে। একসাথে কয়েকটি পূর্ণাঙ্গ অধ্যায় লিখে ফেলবে না।

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
৮. Diagram / উদাহরণ যোগ করা
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
৬. **Technical accuracy।** মূল concept, protocol flow এবং “এটা কী করে” — [`references/`](references/README.md)-এর authoritative source খুলে মিলাও। ব্লগের কথাকে spec-এর উপরে বসাবে না। কোনো নির্দিষ্ট tool-এর প্রতিটি parameter বা internal implementation বুঝে ফেলা এই ধাপের লক্ষ্য নয়।
৭. **Security accuracy।** Threat model, মূল security property, control-এর সীমা ও trade-off যাচাই করো। “JWT secure” এই ধরনের কথা লিখবে না, যদি না সঙ্গে থাকে কোন threat, কোন property, কোথায় সীমা, কী trade-off। নতুন ভুল ধারণা ধরা পড়লে ছোট করে [misconceptions](notes/misconceptions.md)-এ যোগ করো। ব্যাখ্যা অধ্যায়েই থাকবে।
৮. **Diagram বা illustrative example।** [উদাহরণ ও Diagram লেখার নীতি](docs/code-example-policy.md) মেনে চলো। Mermaid flowchart ও sequence diagram হবে প্রধান মাধ্যম; pseudocode default নয়। Go code বা অন্য language-specific implementation code থাকবে না। Tool/library-এর গভীর implementation detail-এ যেও না, যদি না তা core security concept বোঝাতে অপরিহার্য হয়। Diagram অধ্যায়ের ভিতরে রাখো, যদি না সেটি একাধিক অধ্যায়ে পুনর্ব্যবহারের প্রয়োজন হয়।
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

অধ্যায় যখন কোনো বড় security concept শেখায়, এই প্রশ্নগুলো ছুঁয়ে যাও:

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

সব প্রশ্নের উত্তর সমান গভীরতায় দিতে হবে না। অধ্যায়ের মূল উদ্দেশ্যের সঙ্গে সম্পর্কিত বিষয়গুলো বেছে নাও। Tool-specific implementation-এর বিবরণ প্রয়োজন হলে reference-এ পাঠাও, chapter-কে manual বানিও না। উত্তর অধ্যায়ে, authoritative source দেখে লিখো।

## পাঠককে সক্রিয়ভাবে যুক্ত রাখা

এই সিরিজের প্রতিটি অধ্যায় শুধু তথ্য বুঝিয়ে দেবে না, পাঠককে নিজে security decision নিতে এবং নিজের ধারণা যাচাই করতেও সাহায্য করবে। নতুন chapter লেখা বা পুরোনো chapter refine করার সময় এই নিয়মগুলো অনুসরণ করো।

### 🧠 প্রতিটি অধ্যায়ে Decision Game থাকবে

প্রতিটি অধ্যায়ে অন্তত একটি **Decision Game** রাখো। তবে সব অধ্যায়ে একই ধরনের quiz ব্যবহার করবে না। অধ্যায়ের মূল security concept অনুযায়ী game-এর ধরন বেছে নাও:

- **সিদ্ধান্ত বেছে নাও:** কয়েকটি সম্ভাব্য সমাধানের মধ্যে কোনটি আগে করা উচিত?
- **আগে কী ঘটবে অনুমান করো:** code, request flow বা configuration দেখে ফলাফল অনুমান করো।
- **দুর্বলতা খুঁজে বের করো:** ছোট scenario, code বা architecture-এ কোথায় security gap আছে?
- **সঠিক fix বেছে নাও:** একাধিক fix-এর সুবিধা, সীমাবদ্ধতা ও trade-off তুলনা করো।
- **Edge case পরীক্ষা করো:** একটি শর্ত বদলালে আগের সিদ্ধান্ত কি এখনও সঠিক থাকে?
- **Incident investigation:** log বা ঘটনার বিবরণ থেকে কী জানা যায়, কী জানা যায় না, এবং পরের পদক্ষেপ কী হওয়া উচিত তা নির্ধারণ করো।

Game-এর লক্ষ্য কঠিন ধাঁধা বানানো নয়। লক্ষ্য হলো পাঠককে সিদ্ধান্তের পেছনের reasoning করতে শেখানো।

### Game-এর কাঠামো

যেখানে উপযুক্ত, এই ক্রম অনুসরণ করো:

1. **নিজে সিদ্ধান্ত নাও:** বাস্তবসম্মত পরিস্থিতি এবং পরিষ্কার প্রশ্ন দাও।
2. **উত্তর প্রকাশ করো:** GitHub Markdown-এর `<details><summary>...` ব্যবহার করে উত্তর আড়াল রাখা যায়।
3. **কেন ব্যাখ্যা করো:** সঠিক উত্তর কেন কাজ করে, অন্য উত্তরগুলো কেন যথেষ্ট নয়, তা বোঝাও।
4. **Edge case দেখাও:** কোন পরিস্থিতিতে সিদ্ধান্ত বদলাতে পারে, বা সমাধানের সীমা কোথায়, তা বলো।

সব game-এ multiple-choice বাধ্যতামূলক নয়। কখনো পাঠককে vulnerability চিহ্নিত করতে, request flow অনুমান করতে, বা ঘটনার প্রমাণ মূল্যায়ন করতে বলা যায়। উত্তর যেন শুধু মুখস্থ তথ্যের ওপর নির্ভর না করে; chapter-এর ধারণা প্রয়োগ করতে হয়।

### Emoji-কে শেখার সংকেত হিসেবে ব্যবহার করো

Emoji অল্প এবং একই অর্থে ব্যবহার করো। এগুলো যেন পাঠককে section-এর উদ্দেশ্য বুঝতে সাহায্য করে, শুধু সাজসজ্জা না হয়।

- 🧠 **Decision Game:** নিজে ভেবে সিদ্ধান্ত নাও।
- ⚠️ **Security Risk:** সম্ভাব্য ঝুঁকি বা আক্রমণ।
- 🔍 **Investigate:** ঘটনা বা প্রমাণ বিশ্লেষণ করো।
- 🧪 **Edge Case:** অস্বাভাবিক পরিস্থিতি বা সীমা পরীক্ষা করো।
- 💡 **Key Insight:** মনে রাখার মতো মূল শিক্ষা।
- 🛑 **Common Mistake:** প্রচলিত ভুল ধারণা বা ভুল implementation।
- 🎯 **নিজেকে যাচাই করো:** সংক্ষিপ্ত অনুশীলন বা chapter-এর শেষে self-check।

সব emoji প্রতিটি অধ্যায়ে ব্যবহার করতে হবে না। প্রয়োজন অনুযায়ী বেছে নাও। একই অনুচ্ছেদে অনেক emoji দিও না এবং emoji-র ওপর নির্ভর করে গুরুত্বপূর্ণ তথ্য বোঝানোর চেষ্টা করো না।

### Chapter 1-এ ব্যবহৃত engaging technique-গুলো পুনর্ব্যবহার করো

Chapter 1-এ যে পদ্ধতিগুলো পাঠককে সক্রিয়ভাবে ভাবতে সাহায্য করেছে, প্রাসঙ্গিক হলে অন্য chapter-এও ব্যবহার করো:

- **গল্প দিয়ে শুরু:** KenaKata-তে ঘটে যাওয়া নির্দিষ্ট সমস্যা দিয়ে concept পরিচয় করাও। সংজ্ঞা দিয়ে শুরু করা বাধ্যতামূলক নয়।
- **Audit log-এর সীমা দেখানো:** log তদন্তে সহায়ক, কিন্তু IP address বা timestamp একা কোনো মানুষের পরিচয়ের চূড়ান্ত প্রমাণ নয়। অন্য অধ্যায়েও evidence, signal ও proof-এর পার্থক্য স্পষ্ট করো।
- **একটি ছোট, বাস্তবসম্মত Decision Game:** মূল ধারণা বোঝানোর পর পাঠককে একটি নির্দিষ্ট সিদ্ধান্ত নিতে বলো।
- **উত্তর ও reasoning আড়াল করে রাখা:** পাঠক আগে নিজে চেষ্টা করতে পারবে, তারপর ব্যাখ্যা খুলবে।
- **ভুল সমাধান কেন যথেষ্ট নয়:** শুধু সঠিক উত্তর বলবে না; আকর্ষণীয় মনে হওয়া বিকল্পগুলো কেন সমস্যার পূর্ণ সমাধান নয়, সেটাও ব্যাখ্যা করো।
- **Authentication ও Authorization-এর মতো আলাদা concern আলাদা রাখা:** একটি security control কোন কাজ করে এবং কোন কাজ করে না, তা স্পষ্ট করো।
- **ছোট self-check:** chapter শেষে কয়েকটি প্রশ্ন দিয়ে পাঠককে নিজের বোঝাপড়া যাচাই করতে দাও।

এই technique-গুলো checklist হিসেবে যান্ত্রিকভাবে বসাবে না। Chapter-এর বিষয়, গল্প ও পাঠকের সম্ভাব্য ভুল বোঝাবুঝি অনুযায়ী বেছে নাও।

### Diagram, table ও ছোট বিরতি

- Flow বা trust boundary বোঝাতে ছোট **Mermaid diagram** ব্যবহার করো, যখন তা ব্যাখ্যাকে পরিষ্কার করে।
- দুটি ধারণা বা পদ্ধতির পার্থক্য গুরুত্বপূর্ণ হলে **comparison table** ব্যবহার করো।
- HTTP request/response, header, cookie বা token-এর গঠন বোঝাতে প্রয়োজনমতো ছোট illustrative example দাও; runnable implementation code নয়। বিস্তারিত [উদাহরণ ও Diagram লেখার নীতি](docs/code-example-policy.md)-তে।
- বড় ধারণাকে ছোট section-এ ভাগ করো। একটানা দীর্ঘ ব্যাখ্যার বদলে গল্প, diagram, প্রশ্ন এবং ব্যাখ্যার মধ্যে স্বাভাবিক বিরতি রাখো।
- প্রতিটি diagram বা illustrative example-এর সঙ্গে কী লক্ষ্য করতে হবে, তা ব্যাখ্যা করো। শুধু দেখতে সুন্দর বলে visual যোগ করো না।
- অপ্রয়োজনীয় meme, বাহুল্যপূর্ণ decoration বা জোর করে humour যোগ করবে না। হালকা, স্বাভাবিক ভাষা ভালো; technical accuracy ও পাঠকের মনোযোগই অগ্রাধিকার।

### Chapter review: শেখার গভীরতা যাচাই

- পাঠক কি মূল security concept ব্যাখ্যা করতে পারবে, নাকি শুধু algorithm/tool-এর নাম মনে রাখবে?
- উদাহরণটি কি সমস্যা, mechanism, limitation এবং trade-off বোঝায়?
- Tool-specific detail বাদ দিলেও কি পাঠকের mental model অক্ষত থাকে?
- কোনো parameter, implementation choice বা configuration-কে কি সর্বজনীন security rule হিসেবে দেখানো হয়েছে?
- Technical Review ও Security Review কি conceptual correctness, source-backed claims, threat model এবং গুরুত্বপূর্ণ limitation-এ মনোযোগ দিচ্ছে, অপ্রয়োজনীয় implementation-level deep dive-এ নয়?

### Chapter review-এর সময় engagement যাচাই

Chapter শেষ হলে নিজেকে জিজ্ঞেস করো:

- পাঠক কি কোনো জায়গায় নিজে সিদ্ধান্ত নেওয়ার সুযোগ পেয়েছে?
- Decision Game কি chapter-এর মূল concept প্রয়োগ করতে বলছে, নাকি শুধু definition মনে করতে বলছে?
- উত্তরটি কি reasoning-সহ ব্যাখ্যা করা হয়েছে?
- অন্তত একটি গুরুত্বপূর্ণ limitation বা প্রাসঙ্গিক edge case কি পরীক্ষা করা হয়েছে?
- diagram, table ও emoji কি বোঝাপড়া সহজ করছে?
- একই format বারবার ব্যবহার করে chapter-গুলো কি একঘেয়ে হয়ে যাচ্ছে?
- কোনো visual বা game কি technical accuracy-কে সরল করতে গিয়ে ভুল ধারণা তৈরি করছে?

## যা এখানে আসবে না

Site generator, অ্যাপ, package manifest, Go module, বা CI pipeline যোগ করবে না। এই repository Markdown। কোড তখনই, যখন একটা অধ্যায়ের একটা ধারণা বোঝাতে ছোট উদাহরণ লাগে।
