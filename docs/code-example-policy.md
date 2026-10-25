# উদাহরণ ও Diagram লেখার নীতি

এই সিরিজের লক্ষ্য Authentication, Authorization এবং Web Security নিয়ে পাঠকের reasoning তৈরি করা। কোনো নির্দিষ্ট programming language শেখানো বা runnable implementation দেওয়া এই সিরিজের লক্ষ্য নয়। তাই অধ্যায় লেখার সময় নিচের নীতিগুলো মানতে হবে।

## মূল নীতি

1. **Go code বা অন্য কোনো language-specific implementation code ব্যবহার করবে না।** পুরোনো অধ্যায়ে থাকা Go উদাহরণগুলো অধ্যায়ভিত্তিক পরিমার্জনের সময় ধীরে ধীরে সরানো হবে। এই নীতি যোগ হওয়া মানে সব অধ্যায় ইতিমধ্যে বদলে গেছে, এমন নয়।
2. **Mermaid হবে প্রধান visual teaching tool।** কোনো process বা security decision বোঝাতে flowchart ব্যবহার করো। একাধিক actor-এর মধ্যে request/response বা token exchange-এর ক্রম বোঝাতে sequence diagram ব্যবহার করো।
3. **Pseudocode কেবল প্রয়োজন হলে।** Diagram দিয়ে গুরুত্বপূর্ণ logic, condition বা invariant পরিষ্কারভাবে বোঝানো না গেলে অল্প pseudocode ব্যবহার করা যায়। এটি যেন কোনো programming language-এর syntax অনুকরণ করার প্রতিযোগিতা না হয়।
4. **Diagram-কে implementation-এর বিকল্প বলে দেখাবে না।** Diagram বা pseudocode ধারণা শেখানোর জন্য; এগুলো production-ready implementation নয়।
5. **প্রতিটি security-sensitive flow-এর failure path দেখাও।** শুধু সফল পথ নয়, validation ব্যর্থ হলে, permission না থাকলে, session/token expired বা revoked হলে কী হবে, সেটাও প্রাসঙ্গিক জায়গায় দেখাতে হবে।
6. **Security invariant স্পষ্ট করো।** Diagram-এর পাশে বা নিচে ১–৩টি বাক্যে লিখে দাও, কোন শর্তটি সবসময় সত্য থাকতে হবে। যেমন: “Login সফল হওয়ার আগে session authenticated হবে না” অথবা “Resource-এর মালিকানা/permission যাচাই না করে operation চলবে না।”
7. **Protocol ও security claim-এর জন্য authoritative source ব্যবহার করো।** RFC, official standard, browser/vendor documentation বা উপযুক্ত security guidance দেখে দাবি যাচাই করো। Diagram-এ আঁকা হয়েছে বলেই কোনো flow standard-compliant হয়ে যায় না।

## কোন ক্ষেত্রে কী ব্যবহার করবে?

| শেখানোর প্রয়োজন | পছন্দের মাধ্যম |
| --- | --- |
| Login, logout, session lifecycle, permission decision | Mermaid flowchart |
| Browser, application server, authorization server-এর মধ্যে interaction | Mermaid sequence diagram |
| একাধিক বিকল্প বা threat-এর trade-off | Comparison table; দরকার হলে flowchart |
| Diagram-এ বোঝানো কঠিন এবং মূল theory বোঝার জন্য অপরিহার্য decision logic | অল্প, language-neutral pseudocode; অন্য উপায় যথেষ্ট না হলে |
| HTTP request/response, header, cookie বা token-এর গঠন বোঝানো | ছোট illustrative example; runnable implementation নয় |
| নির্দিষ্ট tool/library-এর parameter, parser, API বা implementation detail | সাধারণত বাদ। Core security property ব্যাখ্যার জন্য অপরিহার্য হলে শুধু প্রয়োজনীয় অংশ রাখো এবং authoritative source উল্লেখ করো |

## Diagram লেখার নিয়ম

- Diagram-এর আগে পাঠক কোন প্রশ্নের উত্তর খুঁজবে, সেটা বলো।
- Node ও arrow-এ ছোট, নির্দিষ্ট label ব্যবহার করো। “Validate”-এর মতো অস্পষ্ট label-এর বদলে কী validate হচ্ছে তা লেখো।
- Decision node-এ প্রতিটি branch-এর ফলাফল পরিষ্কার করো, যেমন “হ্যাঁ” / “না”।
- Trust boundary, actor এবং data কোথায় যাচ্ছে তা গুরুত্বপূর্ণ হলে আলাদা করে দেখাও।
- Flowchart-এ failure path বাদ দিও না, তবে অপ্রাসঙ্গিক প্রতিটি edge case-ও এক diagram-এ গুঁজে দিও না। প্রয়োজন হলে diagram ভাগ করো।
- Diagram-এর পরে সেটি কী শেখায়, কী দেখায় না, এবং গুরুত্বপূর্ণ trade-off কী, তা ব্যাখ্যা করো।
- Mermaid rendering না হলেও মূল ব্যাখ্যা যেন বোঝা যায়; diagram-ই যেন একমাত্র তথ্যের উৎস না হয়।

## মূল theory-তে থাকা, tool-specific rabbit hole এড়ানো

প্রতিটি section লেখার আগে জিজ্ঞেস করো: **পাঠক এখান থেকে কোন security concept শিখবে?** উত্তর যদি শুধু কোনো tool-এর configuration মুখস্থ করা হয়, section-টি ছোট করো বা বাদ দাও।

- Concept: কী সমস্যা হচ্ছে, এবং কেন?
- Mechanism: security control কীভাবে সমস্যাটি কমায়?
- Boundary: এটি কী সমাধান করে না?
- Threat: কোন আক্রমণ বা failure mode এখনও গুরুত্বপূর্ণ?
- Trade-off: user experience, performance, availability বা complexity-তে কী দাম দিতে হয়?
- Practical connection: engineer বাস্তব architecture-এ কোন সিদ্ধান্ত নিতে পারবে?

কোনো tool-specific detail কেবল তখনই রাখো, যখন সেটি ভুল security assumption ঠেকাতে বা মূল mechanism বোঝাতে সত্যিই দরকার। বিস্তারিত reference-এ পাঠানো যেতে পারে; chapter-কে reference manual বানিও না।

## Chapter review checklist

- [ ] উদাহরণটি কি মূল security concept শেখাচ্ছে, নাকি tool-specific implementation detail-এ ডুবে যাচ্ছে?
- [ ] Mermaid flowchart বা sequence diagram কি logic-টি আরও পরিষ্কারভাবে বোঝাতে পারে?
- [ ] গুরুত্বপূর্ণ failure path ও security boundary কি দেখানো হয়েছে?
- [ ] Security invariant কি স্পষ্ট?
- [ ] Tool-specific detail বাদ দিলেও কি পাঠক core theory বুঝতে পারবে?
- [ ] কোনো algorithm বা library-র parameter-কে কি ভুল করে universal rule হিসেবে দেখানো হচ্ছে?
- [ ] Review-তে কি implementation-level exhaustive validation-এর বদলে chapter-এর claims, threat model ও conceptual accuracy-তে অগ্রাধিকার দেওয়া হয়েছে?
- [ ] Diagram বা pseudocode থেকে কি পাঠক ভুল security assumption করতে পারে?
- [ ] উদাহরণটি কি production-ready বলে ভুল বোঝার সুযোগ আছে?
- [ ] Protocol বা standard-সংক্রান্ত দাবি কি authoritative source দিয়ে যাচাই করা হয়েছে?

এই নীতির ভিত্তিতে chapter-গুলো একবারে একটি করে আপডেট হবে। পুরোনো chapter-এ code থাকলে সেটিকে স্বয়ংক্রিয়ভাবে সঠিক বা নীতিসম্মত ধরে নেওয়া যাবে না।
