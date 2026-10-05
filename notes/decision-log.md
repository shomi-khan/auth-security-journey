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
- Examples: দরকার হলে Go। ছোট, একটা ধারণা। পুরো backend নয়
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
