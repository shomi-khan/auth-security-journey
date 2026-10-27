# Contributing

এই repository-তে বাংলা ভাষায় Authentication, Authorization ও Web Security শেখানো হয়। লক্ষ্য হলো পাঠকের security reasoning তৈরি করা, কোনো framework বা library-র implementation manual লেখা নয়।

## Chapter লেখার workflow

1. পরের planned chapter বেছে নাও; একবারে একটি chapter-এ কাজ করো।
2. [`product/`](product/README.md) এবং [`product/timeline.md`](product/timeline.md) পড়ো। কোন feature গল্পে কখন আসে, তা timeline নির্ধারণ করে।
3. আগের ও পরের chapter-এর সঙ্গে মিলিয়ে problem, threat model এবং শেখার লক্ষ্য ঠিক করো।
4. সংশ্লিষ্ট [authoritative references](references/README.md) দেখে technical ও security claim যাচাই করো।
5. Chapter লিখে diagram, Decision Game, self-check এবং cross-reference যাচাই করো।
6. প্রয়োজনীয় canonical product document, season index ও root README update করো।

## KenaKata-র ধারাবাহিকতা

- Canonical product facts থাকবে [`product/`](product/README.md)-এ। গল্পের প্রয়োজন বলে নতুন character, feature বা architecture চুপিচুপি যোগ কোরো না।
- [`product/timeline.md`](product/timeline.md) বলে কোন অধ্যায়ে কোন feature আগে থেকে আছে। [`product/architecture.md`](product/architecture.md) হলো শেষ অবস্থার architecture, শুরুর system নয়।
- ক্রেতা customer record; এই সিরিজে customer login করে না।
- Role থাকে membership-এ। Team আলাদা role দেয় না।
- Google login, external application, MFA ও microservice timeline-এ আসার আগে product-এর চালু feature নয়।

## লেখার মান

- বাংলা স্বাভাবিক ও প্রাঞ্জল রাখো; technical term সাধারণত ইংরেজিতে রাখো। জোর করে অনুবাদ বা অপ্রয়োজনীয় jargon ব্যবহার কোরো না।
- বাস্তব সমস্যা দিয়ে concept পরিচয় করাও। পাঠককে বোঝাও কেন control দরকার, কীভাবে কাজ করে, কী আটকায় না এবং কী trade-off তৈরি করে।
- Chapter-এর শুরুতে ২–৪টি learning outcome দাও।
- প্রতিটি chapter-এ অন্তত একটি প্রাসঙ্গিক **Decision Game** রাখো। উত্তর প্রকাশ করলে সঠিক সিদ্ধান্তের reasoning, আকর্ষণীয় ভুল বিকল্পের সীমা এবং প্রাসঙ্গিক edge case ব্যাখ্যা করো।
- Self-check-এর প্রতিটি প্রশ্নের সঙ্গে একই নম্বরের উত্তর মিলতে হবে। কোনো প্রশ্ন যেন উত্তরহীন না থাকে; উত্তর যেন অন্য প্রশ্নের উত্তর না হয়ে যায়।
- Diagram-এর আগে কী দেখতে হবে বলো, পরে তার মূল inference ব্যাখ্যা করো। Security-sensitive flow-তে প্রাসঙ্গিক failure path ও invariant দেখাও।
- Mermaid flowchart/sequence diagram প্রধান visual tool। Go বা অন্য language-specific implementation code নয়; pseudocode কেবল diagram যথেষ্ট না হলে। বিস্তারিত [code ও diagram policy](docs/code-example-policy.md)-তে।
- Technical claim-এর জন্য বর্তমান RFC, specification, browser/vendor documentation বা উপযুক্ত security guidance দেখো। Conceptual example-কে tested behavior বলে উপস্থাপন কোরো না।
- Authentication, session, CSRF, XSS, token validation ও authorization-কে একে অন্যের বিকল্প হিসেবে দেখিও না।

## Chapter checklist

- [ ] Learning outcomes ও মূল প্রশ্ন পরিষ্কার।
- [ ] গল্প ও product facts timeline-এর সঙ্গে মেলে।
- [ ] Technical/security claim authoritative source দিয়ে যাচাই করা।
- [ ] Diagram ও prose একই flow বোঝায়; গুরুত্বপূর্ণ failure path ব্যাখ্যা করা।
- [ ] Decision Game-এর উত্তর reasoning-সহ আছে।
- [ ] Self-check-এ প্রশ্ন ও উত্তর নম্বর, সংখ্যা এবং অর্থে মেলে।
- [ ] আগের/পরের chapter, references এবং relative links ঠিক আছে।
- [ ] Front matter-এর `status` ও প্রয়োজনীয় chapter index update করা হয়েছে।
- [ ] Published না হলে প্রকাশিত বলে দাবি করা হয়নি।

Chapter-এর গুরুত্বপূর্ণ পরিবর্তন হলে সংশ্লিষ্ট canonical document ও index update করো। পুরোনো review history বা branch-specific status note রাখার প্রয়োজন নেই; বর্তমান সত্য তথ্য তার প্রাসঙ্গিক document-এই থাকবে।
