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
| Diagram-এ বোঝানো কঠিন ছোট decision logic | অল্প pseudocode, সঙ্গে ব্যাখ্যা |
| HTTP request/response, header, cookie বা token-এর গঠন বোঝানো | ছোট illustrative example; runnable implementation নয় |
| নির্দিষ্ট language/framework-এর implementation detail | সাধারণত বাদ। Security concept বোঝার জন্য অপরিহার্য হলে language-neutral ব্যাখ্যা ও authoritative reference ব্যবহার করো |

## Diagram লেখার নিয়ম

- Diagram-এর আগে পাঠক কোন প্রশ্নের উত্তর খুঁজবে, সেটা বলো।
- Node ও arrow-এ ছোট, নির্দিষ্ট label ব্যবহার করো। “Validate”-এর মতো অস্পষ্ট label-এর বদলে কী validate হচ্ছে তা লেখো।
- Decision node-এ প্রতিটি branch-এর ফলাফল পরিষ্কার করো, যেমন “হ্যাঁ” / “না”।
- Trust boundary, actor এবং data কোথায় যাচ্ছে তা গুরুত্বপূর্ণ হলে আলাদা করে দেখাও।
- Flowchart-এ failure path বাদ দিও না, তবে অপ্রাসঙ্গিক প্রতিটি edge case-ও এক diagram-এ গুঁজে দিও না। প্রয়োজন হলে diagram ভাগ করো।
- Diagram-এর পরে সেটি কী শেখায়, কী দেখায় না, এবং গুরুত্বপূর্ণ trade-off কী, তা ব্যাখ্যা করো।
- Mermaid rendering না হলেও মূল ব্যাখ্যা যেন বোঝা যায়; diagram-ই যেন একমাত্র তথ্যের উৎস না হয়।

## Chapter review checklist

- [ ] উদাহরণটি কি security concept শেখাচ্ছে, নাকি অপ্রয়োজনীয় implementation syntax দেখাচ্ছে?
- [ ] Mermaid flowchart বা sequence diagram কি logic-টি আরও পরিষ্কারভাবে বোঝাতে পারে?
- [ ] গুরুত্বপূর্ণ failure path ও security boundary কি দেখানো হয়েছে?
- [ ] Security invariant কি স্পষ্ট?
- [ ] Diagram বা pseudocode থেকে কি পাঠক ভুল security assumption করতে পারে?
- [ ] উদাহরণটি কি production-ready বলে ভুল বোঝার সুযোগ আছে?
- [ ] Protocol বা standard-সংক্রান্ত দাবি কি authoritative source দিয়ে যাচাই করা হয়েছে?

এই নীতির ভিত্তিতে chapter-গুলো একবারে একটি করে আপডেট হবে। পুরোনো chapter-এ code থাকলে সেটিকে স্বয়ংক্রিয়ভাবে সঠিক বা নীতিসম্মত ধরে নেওয়া যাবে না।
