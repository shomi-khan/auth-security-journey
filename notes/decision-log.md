# সিদ্ধান্তের খাতা

বড় সম্পাদনার বা product-এর সিদ্ধান্ত এখানে। পরে কোনো অধ্যায় গল্প, ভাষা, বা কাঠামো বদলালে তারিখ দিয়ে নতুন এন্ট্রি যোগ করো। পুরনো এন্ট্রি মুছে দিয়ো না।

## 2026-10-05 — শুরুর সিদ্ধান্ত

- Repository: `auth-security-journey`
- Target audience: বাংলাভাষী junior software engineer
- Primary language: বাংলা। পাঠকের জন্য README, season, chapter, `product/`, `references/`, আর `notes/` বাংলা
- Technical terminology: ইংরেজি। “প্রমাণীকরণ” বা “অনুমোদন”-এর মতো জোর করা অনুবাদ নয়
- Fictional product: KenaKata
- Total chapters: 25
- Seasons: 3
- Teaching style: আগে সমস্যা, তারপর সমাধান
- Narrative style: একটা product-এর যাত্রা
- Examples: Mermaid flowchart/sequence diagram প্রধান; pseudocode শুধু প্রয়োজন হলে। Go code বা অন্য language-specific implementation code নয়। বিস্তারিত [উদাহরণ ও Diagram লেখার নীতি](../docs/code-example-policy.md)।
- Security claims: authoritative source দেখে যাচাই করতে হবে। সোর্স কী বলছে, সেটা এই খাতায় তুলে রাখা হয়নি
- Chapters: একবারে একটি করে লেখা হবে

গল্প যাতে না ভাঙে, সেই জন্য আরও কয়েকটা সিদ্ধান্ত:

- ক্রেতা customer record। এই সিরিজে ক্রেতা login করে না। User মানে মালিক আর কর্মী।
- Role চারটা: Owner, Admin, Member, Viewer। Role থাকে membership-এ। Team মানুষকে দল করে, দ্বিতীয় role-ব্যবস্থা নয়।
- সরল মডেলে একটা organization-এ একজন Owner।
- আলাদা platform-superadmin চরিত্র নেই।
- নিয়মিত চরিত্র: আরিফ, নীরা, রাফি।
- [timeline](../product/timeline.md) product-এর কালক্রম। Feature আছে কি না, সেই প্রশ্নে অধ্যায়ের ক্রমের উপরে এই ফাইল।
- [architecture](../product/architecture.md) শুধু শেষ অবস্থা।
- MFA কোন উপায়ে হবে, সেটা ধরা নেই।
- দাম আর প্ল্যানের নাম ধরা নেই।
- লাইসেন্স MIT। Copyright-এর জায়গায় `[COPYRIGHT HOLDER]`, কারণ মালিকের নাম দেওয়া হয়নি।


## 2026-10-24 — অধ্যায় ৫-এর session design

এই সিদ্ধান্তগুলো Chapter 5-এর KenaKata-দৃশ্য ও উদাহরণে গৃহীত আচরণ। এগুলো সর্বজনীন security policy নয়।

- Login-এর আগে anonymous session থাকবে, যাতে login-এর পর user-কে তার চাওয়া পাতায় ফেরানো যায়।
- Session-এর তিনটি server-side timeout: anonymous session ৩০ মিনিট, authenticated session-এর idle timeout ৩০ মিনিট, absolute timeout login থেকে ৮ ঘণ্টা।
- Login সফল হলে session ID rotate হবে; password বদলালে বর্তমান session-এর ID-ও rotate হবে এবং ওই user-এর অন্য session বাতিল হবে।
- Session record-এ login-এর সময়ের User-Agent ও network prefix রাখা হবে। বদলকে risk signal হিসেবে ধরা হবে, পরিচয়ের প্রমাণ হিসেবে নয়; KenaKata-র ছোট দোকানের দৃশ্যের বাইরে এই policy পুনর্বিবেচনা করতে হবে।
- Session store-এর `Lookup` পুরো record ফেরত দেবে, যাতে authentication state, timeout ও context signal যাচাই করা যায়।
- শুভর session কীভাবে চুরি হয়েছে তা গল্পে নির্দিষ্ট করা হবে না; কারণ চুরির পথ জানা না থাকলেও incident response ও ক্ষতি সীমিত করার ব্যবস্থা দরকার।


## 2026-10-25 — উদাহরণ ও Diagram-এর নীতি

- সিরিজটি কোনো নির্দিষ্ট programming language শেখানোর জন্য নয়; Go code এবং অন্য language-specific implementation code বাদ থাকবে।
- Mermaid flowchart হবে process ও security decision বোঝানোর প্রধান মাধ্যম; actor-দের interaction ও token exchange বোঝাতে Mermaid sequence diagram ব্যবহার করা হবে।
- Pseudocode কেবল তখনই ব্যবহার করা যাবে, যখন diagram দিয়ে গুরুত্বপূর্ণ logic বা condition পরিষ্কার করা কঠিন।
- Security-sensitive উদাহরণে প্রাসঙ্গিক failure path ও security invariant স্পষ্ট করতে হবে।
- Diagram ও pseudocode শিক্ষামূলক মডেল, production-ready implementation নয়।
- Protocol ও security claim authoritative source দিয়ে যাচাই করতে হবে।
- পুরোনো অধ্যায়ের code একবারে সব বদলানো হবে না; chapter-by-chapter আপডেট করা হবে।
- বিস্তারিত নিয়ম: [উদাহরণ ও Diagram লেখার নীতি](../docs/code-example-policy.md)।
