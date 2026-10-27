# পাঠককেন্দ্রিক লেখার নির্দেশিকা

এই নথি `trust-stack`-এর বাংলা Authentication ও Security সিরিজকে প্রাঞ্জল, নির্ভুল, চিত্রসমর্থিত এবং শেখার জন্য আনন্দদায়ক রাখার editorial standard। এটি কোনো chapter-এর technical review বা production security audit-এর বিকল্প নয়।

## প্রতিটি অধ্যায়ের পাঠ-অভিজ্ঞতা

1. **বাস্তব সমস্যা দিয়ে শুরু:** গল্প বা incident যেন মূল security প্রশ্নের দিকে নিয়ে যায়। গল্পের চরিত্র/ঘটনা যেন ধারণার বিকল্প না হয়।
2. **শেখার লক্ষ্য আগে:** পাঠক অধ্যায় শেষে কী ব্যাখ্যা করতে বা সিদ্ধান্ত নিতে পারবে, ২–৪টি bullet-এ জানাও।
3. **একবারে একটি নতুন ধারণা:** নতুন technical term প্রথমবার এলে সহজ বাংলা সংজ্ঞা দাও; দরকারি English term code font-এ রাখো।
4. **কারণ থেকে ফল:** শুধু “এটা করো” নয়, কেন দরকার, কোন threat কমায়, কী trade-off এবং কী আটকায় না, তা বোঝাও।
5. **চিত্রের সঙ্গে লেখা:** প্রতিটি diagram-এর আগে পাঠককে কী দেখতে হবে বলো, আর পরে মূল inference ব্যাখ্যা করো। Diagram-এ থাকা গুরুত্বপূর্ণ branch বা failure path লেখায় ব্যাখ্যা করা চাই।
6. **বাস্তব উদাহরণ:** request/response, ছোট code, state transition বা incident-এ ধারণাটি প্রয়োগ করো। Code থাকলে সেটি runnable কি না, illustrative কি না, স্পষ্ট বলো।
7. **ভুল ধারণা ধরো:** কাছাকাছি concept-এর পার্থক্য দেখাও; বিশেষ করে authentication বনাম authorization, CORS বনাম CSRF, expiry বনাম revocation, token validation বনাম permission check।
8. **নিজে যাচাই:** প্রশ্নগুলো যেন মুখস্থ সংজ্ঞা নয়, scenario-ভিত্তিক reasoning পরীক্ষা করে। যেখানে কাজে লাগে, collapsible answer guide দাও। উত্তরের ব্যাখ্যা প্রশ্নের নম্বরের সঙ্গে মিলতে হবে।
9. **পরের অধ্যায়ের সেতু:** শেষের প্রশ্ন পরের অধ্যায়ের বিষয়কে স্বাভাবিকভাবে তৈরি করবে, তবে একই ব্যাখ্যা আবার পুরোপুরি পুনরাবৃত্তি করবে না।

## প্রাঞ্জল বাংলার নিয়ম

- ছোট, স্বাভাবিক বাক্য ব্যবহার করো। এক বাক্যে একাধিক নতুন technical claim গুঁজে দিও না।
- বাংলা বাক্যের মধ্যে পরিচিত English technical term রাখা যায়; একই ধারণার জন্য বারবার আলাদা বাংলা অনুবাদ তৈরি কোরো না।
- “সহজ”, “স্পষ্ট” বা “নিরাপদ” বলেই থেমো না। কী কারণে সহজ/স্পষ্ট/নিরাপদ, তা দেখাও।
- অপ্রয়োজনীয় নাটকীয়তা, একই কথা বারবার বলা এবং অতিরিক্ত emoji কমাও।
- উপমা ব্যবহার করলে তার সীমা বলো; security concept-কে উপমার সঙ্গে পুরোপুরি এক করে দেখিও না।
- কোনো দাবির certainty source বা architecture-এর চেয়ে বেশি করে দেখিও না। “সবসময়” বা “অসম্ভব” বলার আগে ব্যতিক্রম যাচাই করো।

## Diagram ও figure-এর মানদণ্ড

- Diagram একটি নির্দিষ্ট প্রশ্নের উত্তর দেবে: request flow, trust boundary, state transition, threat path অথবা decision tree।
- Diagram-এর প্রতিটি node/arrow-এর অর্থ পাঠ্যাংশে ব্যাখ্যা করতে হবে।
- Success path-এর পাশাপাশি গুরুত্বপূর্ণ failure path-ও দেখাও।
- Diagram-এ protocol-এর আচরণ ও application-এর policy গুলিয়ে ফেলো না। উদাহরণস্বরূপ, CORS preflight অনুমোদন authentication বা authorization-এর প্রমাণ নয়।
- Diagram-কে “conceptual” বললে প্রয়োজনে সেটি বাস্তব protocol-এর পূর্ণ flow নয়, তা নির্দিষ্ট করে দাও।
- Mermaid diagram-এ node label সহজ রাখো, যেন GitHub rendering-এ parse error না হয়।
- Diagram পুনরাবৃত্তি করে শুধু সাজসজ্জা বাড়িও না। একটি পরিষ্কার চিত্র ও ব্যাখ্যা একাধিক প্রায়-একই চিত্রের চেয়ে ভালো।

## Security accuracy checklist

- Authentication, session management, authorization ও resource ownership আলাদা করে দেখানো হয়েছে?
- Attacker কী জানে, কী নিয়ন্ত্রণ করে এবং কী করতে পারে, তা স্পষ্ট?
- Control কোন threat কমায় এবং কোন threat কমায় না, তা বলা হয়েছে?
- Browser behavior, HTTP standard এবং application-specific policy আলাদা রাখা হয়েছে?
- RFC/OWASP/NIST reference-এর scope ঠিক আছে? OAuth-specific rule-কে first-party custom flow-এর requirement হিসেবে ভুলভাবে দেখানো হয়নি?
- Token/session expiry, revocation, logout ও authorization cache-এর delay সম্পর্কে অতিরিক্ত নিশ্চয়তা দেওয়া হয়নি?
- Example request, status code, diagram এবং prose পরস্পরের সঙ্গে সামঞ্জস্যপূর্ণ?
- Practice question-এর answer guide প্রশ্নের সঙ্গে মেলে, এবং নতুন ভুল ধারণা তৈরি করে না?
- Chapter links, chapter numbers ও series roadmap বর্তমান outline-এর সঙ্গে মেলে?

## Editorial status কী বোঝায়

- **Draft:** লেখা আছে, কিন্তু প্রকাশের জন্য প্রস্তুত বলে ধরে নেওয়া যাবে না।
- **Technical review:** নির্দিষ্ট technical claim ও reference পর্যালোচিত; scope উল্লেখ থাকতে হবে।
- **Security review:** threat model, limitation ও failure path পর্যালোচিত; এটি production certification নয়।
- **Published:** নির্ধারিত editorial ও technical release criteria পূরণ হয়েছে।

শুধু লেখা সুন্দর হয়েছে বলে Technical Review, Security Review বা Published checkbox টিক দেওয়া যাবে না। Review-এর সীমা ও যাচাই না-করা বিষয় পাঠকের কাছে সৎভাবে জানাতে হবে।
