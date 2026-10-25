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


## 2026-10-25 — Chapter 1-এ diagram-first পদ্ধতির pilot

- Chapter 1 থেকে Go implementation snippet সরিয়ে Mermaid flowchart ও sequence diagram দিয়ে route exposure, server-side authentication guard, protected route grouping এবং প্রতি request-এ Basic credential যাচাইয়ের flow বোঝানো হয়েছে।
- Browser-এর বাইরে থেকেও request পাঠানো যায়, সেটি বোঝাতে runnable command-এর বদলে illustrative HTTP request রাখা হয়েছে।
- Log sample, HTTP request ও protocol header-এর মতো ছোট illustrative example রাখা যাবে; এগুলো implementation code নয়।
- এটি প্রথম pilot। Chapter 1-এর technical ও security review এখনও আলাদা করে সম্পন্ন করতে হবে; code test না চালানোকে correctness-এর প্রমাণ ধরা যাবে না।


## 2026-10-25 — Chapter 1 final polish after diagram-first pilot

- Chapter 1-এর Basic Authentication trade-off-এ credential কোন component-এ পৌঁছাতে পারে, তা আরও নির্দিষ্টভাবে লেখা হয়েছে; TLS terminate হওয়ার পর সব server-side component স্বয়ংক্রিয়ভাবে password পায়, এমন দাবি করা হয়নি।
- Basic Authentication-এর limitation-কে session-level revocation-এর অভাব হিসেবে নির্দিষ্ট করা হয়েছে; credential disable/change এবং individual session revocation এক করে দেখানো হয়নি।
- Authentication সফল হলেও authorization check আলাদা, সেটি Basic Authentication flow-তে দৃশ্যমান করা হয়েছে।
- পুনরাবৃত্ত Basic Authentication sequence diagram সরিয়ে একটি diagram-এ credential failure, authorization denial ও success-এর পার্থক্য রাখা হয়েছে।
- Chapter 1-এর Technical Review ও Security Review checkbox এখনো unchecked। এই পরিবর্তনগুলো final polish, formal sign-off নয়।


## 2026-10-25 — Chapter 2-এ diagram-first পদ্ধতির প্রয়োগ

- Chapter 2 থেকে Go implementation snippet ও template code সরিয়ে Mermaid flowchart, sequence diagram, permission matrix এবং illustrative HTTP/log examples ব্যবহার করা হয়েছে।
- Function-level authorization (BFLA) ও object-level authorization (BOLA) আলাদা করে দেখানো হয়েছে; client-provided target ID-কে server-verified identity ধরে নেওয়া যাবে না।
- Server-side authorization decision-এ subject, action ও resource-এর সম্পর্ক, deny-by-default, least privilege, প্রতিটি protected request-এ check এবং denial logging-এর ভূমিকা রাখা হয়েছে।
- Route-টি শুধু POST method গ্রহণ করবে এবং POST থাকলেও CSRF আলাদা প্রতিরক্ষা, এই বিষয়টি language-specific example ছাড়াই রাখা হয়েছে।
- এটি teaching-style update; chapter-এর Technical Review ও Security Review checkbox এখনো unchecked। Code test চালানো হয়নি এবং এই পরিবর্তনকে security certification ধরা যাবে না।


## 2026-10-25 — Chapter 3-এ diagram-first পদ্ধতির প্রয়োগ

- Chapter 3 থেকে Go implementation snippet, SQL snippet এবং runnable benchmark সরানো হয়েছে। Password storage, fast hash বনাম Argon2id, bcrypt-এর ৭২-byte সীমা, salt, PHC string, dummy-hash login flow এবং password policy Mermaid diagram বা language-neutral illustrative text দিয়ে বোঝানো হয়েছে।
- বাস্তব implementation-এর ফল হিসেবে কোনো benchmark বা code test দাবি করা হচ্ছে না। Hardware-dependent Argon2id tuning-এর জন্য বাস্তব system-এ benchmark প্রয়োজন, কিন্তু এই learning series-এর উদাহরণগুলো runnable implementation নয়।
- এই entry লেখার সময় Chapter 3-এর Technical Review ও Security Review checkbox unchecked ছিল। পরবর্তী source-based review entry-তে এগুলোর বর্তমান অর্থ ও অবস্থা নথিভুক্ত হয়েছে.


## 2026-10-25 — Theory-first security teaching

- সিরিজের প্রধান লক্ষ্য junior engineer-দের security theory, mental model, threat model, security control-এর সীমা এবং trade-off বোঝানো। কোনো নির্দিষ্ট tool, library বা implementation গভীরভাবে শেখানো নয়।
- Chapter-এর প্রতিটি section-এ core learning objective থাকতে হবে: সমস্যা কী, mechanism কেন কাজ করে, কী সমাধান করে না, এবং কী trade-off থাকে।
- Algorithm, library ও standard-এর নাম প্রয়োজনে থাকবে, কিন্তু parameter list, parser detail, API usage বা implementation edge case তখনই থাকবে যখন তা core concept বোঝাতে অপরিহার্য বা ভুল security assumption ঠেকাতে দরকার।
- Technical Review-এর লক্ষ্য chapter-এর মূল claims, protocol/concept accuracy, source quality ও mental model যাচাই করা। Security Review-এর লক্ষ্য threat model, important risks, control boundaries এবং misleading security assurance যাচাই করা। Production implementation certification বা exhaustive library audit এই blog series-এর review scope নয়।
- Mermaid diagram, story, language-neutral example এবং decision game প্রধান teaching tools। Pseudocode default নয়; language-specific implementation code থাকবে না।
- Chapter 3-এ Argon2id parameter table, exact encoded hash example, bcrypt input-length deep dive ও Unicode code-point detail সংক্ষিপ্ত করে মূল theory-তে ফোকাস করা হয়েছে।


## 2026-10-25 — Chapter 4: Session and Cookie theory-first revision

- Chapter 4-এ Go implementation snippets ও executable shell commands বাদ দিয়ে Mermaid flowchart/sequence diagram এবং language-neutral HTTP examples ব্যবহার করা হবে।
- মূল teaching objective: authentication সফল হওয়ার পর session কীভাবে continuity ধরে রাখে; cookie কীভাবে browser-এ credential বহন করে; server-side session state কীভাবে lookup, expiry ও revocation সমর্থন করে; cookie attributes কী ঝুঁকি কমায় এবং কোন ঝুঁকি থেকে যায়।
- Framework-specific code, exact library API, compile/test history ও implementation-only cleanup detail chapter-এর teaching content নয়। এগুলো দরকার হলে আলাদা implementation guide-এ রাখা যাবে।
- Security claims-এর review-তে cookie attributes, session ID entropy, replay, fixation, expiry, logout, session store leakage এবং browser-vs-server responsibility যাচাই করতে হবে। এই entry-র পরের Chapter 3 ও 4 discrepancy review-তে Chapter 4-এর source-based review-এর ফল নথিভুক্ত হয়েছে.

## 2026-10-25 — Chapter 3 ও 4 discrepancy review

- Chapter 3 ও 4-এর branch-এর বর্তমান সংস্করণ যাচাই করা হয়েছে। দুটো অধ্যায়েই language-specific implementation code নেই; Mermaid diagram ও language-neutral/HTTP illustrative example ব্যবহার করা হয়েছে।
- Chapter 3-এর password storage, salt, offline guessing ও Argon2id-সংক্রান্ত মূল দাবিগুলো OWASP Password Storage Cheat Sheet এবং NIST SP 800-63B-4-এর সঙ্গে source-basedভাবে মিলিয়ে দেখা হয়েছে। NIST-এর বর্তমান নির্দেশনায় password blocklist ও Unicode normalization-সংক্রান্ত দাবি আছে; Unicode-এর মূল specification আলাদা করে পুরো পড়া হয়েছে বলে দাবি করা হচ্ছে।
- Chapter 4-এর session ID entropy/CSPRNG, session store-এ verifier hash, cookie বনাম session, replay ও browser/server responsibility-র মূল দাবিগুলো OWASP Session Management Cheat Sheet এবং RFC/MDN-এ বর্ণিত cookie behavior-এর সঙ্গে মিলিয়ে দেখা হয়েছে।
- Chapter 4-এ কয়েকটি ভাষাগত/ধারণাগত discrepancy ঠিক করা হয়েছে: Argon2id latency-কে সর্বজনীন benchmark হিসেবে না দেখানো, language-specific random-generator নামের বদলে CSPRNG বলা, এবং hashed session verifier-সহ read-only store disclosure-কে “শূন্য ক্ষতি” না বলা।
- Chapter 3 ও 4-এর Technical Review ও Security Review checkbox-কে conceptual/source-based review-এর অর্থে রাখা হয়েছে। এগুলো implementation audit, automated test, বা production security certification বোঝায় না।
- এই review-তে কোনো application code execute বা implementation test করা হয়নি। পরবর্তী cross-chapter review-তে Chapter 1-এর credential/session revocation wording, Chapter 4 ও 5-এর timeout policy, Chapter 5-এর language-specific code এবং stale test/implementation claims পরীক্ষা ও সংশোধন করা হয়েছে.


## 2026-10-25 — Cross-chapter consistency review (Chapters 1–5)

- Chapter 1-এর comparison table-এ password বদলানো মানেই সব চালু session স্বয়ংক্রিয়ভাবে বাতিল হয়, এমন অতিরঞ্জিত ধারণা সরানো হয়েছে। Credential পরিবর্তন এবং authentication state/session revocation আলাদা policy decision হিসেবে স্পষ্ট করা হয়েছে।
- Chapter 4-এর session expiry অংশে সাধারণ নীতি ও KenaKata-র নির্দিষ্ট policy আলাদা করা হয়েছে। Chapter 5-এর illustrative policy: anonymous session ৩০ মিনিট, authenticated idle timeout ৩০ মিনিট, absolute timeout login থেকে ৮ ঘণ্টা। এগুলো KenaKata-র গল্পের সিদ্ধান্ত, সব application-এর universal default নয়।
- Chapter 5 থেকে Go implementation ও shell command tutorial সরানো হয়েছে। Timeout, revocation, context-signal, fixation এবং rotation-এর mechanism Mermaid diagram, language-neutral explanation ও conceptual review question-এ রূপান্তরিত হয়েছে। পুরোনো implementation-test চালানো হয়েছে বলে এই সংস্করণকে দাবি করা হচ্ছে না।
- Chapter 5-এর implementation-specific function/field reference যতটা সম্ভব সাধারণ ধারণায় বদলানো হয়েছে; proxy header, session context, safe redirect destination এবং rotation failure-এর গুরুত্বপূর্ণ security boundary রাখা হয়েছে।
- Chapter 5-এ OWASP WSTG fixation-testing-এর latest development guide এবং stable v4.2 guide আলাদা করে চিহ্নিত করা হয়েছে। Cookie Theft Mitigation source references/owasp.md-তে আছে; chapter-এর পুরোনো “এখনো নেই” নোট সরানো হয়েছে.
- Chapter 5-এর source-based Technical Review ও Security Review সম্পন্ন হয়েছে এবং chapter checkbox update করা হয়েছে। এটি conceptual claims, authoritative references, threat model, control boundaries ও failure paths-এর review; implementation audit, automated test বা production security certification নয়.
- PR open রাখা হয়েছে; merge করা হয়নি। Application code execute বা automated tests চালানো হয়নি।

## 2026-10-25 — Chapter 5 source-based Technical and Security Review

- Chapter 5-এর সংজ্ঞা, session hijacking বনাম fixation-এর পার্থক্য, timeout model, revocation, context signal, session rotation এবং DBSC-সংক্রান্ত মূল দাবিগুলো OWASP Session Management Cheat Sheet, OWASP Cookie Theft Mitigation Cheat Sheet, OWASP WSTG, NIST SP 800-63B-4 এবং cookie standard-এর সঙ্গে মিলিয়ে পর্যালোচনা করা হয়েছে।
- Cookie standard-এর ক্ষেত্রে RFC 10025 (July 2026) বর্তমান standard; RFC 6265-কে historical context হিসেবে রাখা হয়েছে। RFC 10025-এর Security Considerations-এ cookie signing/encryption থাকলেও cookie transplant ও replay আটকায় না, এবং session fixation-এর তিন ধাপের ব্যাখ্যাও আছে।
- Log-এ session ID-র প্রথম আট অক্ষরের মতো ছোট correlation value-কে নিরাপদ ধরে নেওয়ার ইঙ্গিত সরানো হয়েছে। উদাহরণে raw session ID-এর বদলে keyed HMAC-ভিত্তিক দীর্ঘ correlation value ব্যবহার করার নীতি লেখা হয়েছে; বাস্তব log design-এ secret management, retention ও access control আলাদাভাবে দরকার।
- Renewal timeout-এর ব্যাখ্যা সংশোধন করা হয়েছে: পুরোনো ID অনির্দিষ্টভাবে বৈধ রাখা নিরাপদ সমাধান নয়; concurrent in-flight request-এর কারণে retry বা reauthentication লাগতে পারে।
- Password change-এর failure path স্পষ্ট করা হয়েছে: বর্তমান session rotate হবে, অন্য session বাতিল হবে; rotation/revocation state নিশ্চিত না হলে পুরোনো session সচল না রেখে সব session invalidate করে নতুন login চাইতে হবে।
- NIST-এর overall reauthentication timeout ও application-এর absolute session timeout এক জিনিস নয়, এই পার্থক্য যোগ করা হয়েছে। DBSC-র hardware protection-কে platform-dependent হিসেবে qualify করা হয়েছে; এটিকে ongoing compromised-device protection হিসেবে দেখানো হয়নি।
- OWASP WSTG-এর latest development guide এবং versioned stable v4.2 guide আলাদা করা হয়েছে। RFC ও OWASP-এর বর্তমান reference path মিলিয়ে নেওয়া হয়েছে।
- Review conceptual/source-based। Application code, automated tests, browser matrix বা production configuration execute/verify করা হয়নি; তাই এই review production security certification নয়।

## 2026-10-25 — Chapter 6: CSRF, theory-first revision

- Chapter 6-এ Go implementation ও runnable shell command বাদ দিয়ে Mermaid diagram, illustrative HTTP request, route-policy table এবং manual review checklist রাখা হয়েছে। Source-based conceptual review নথিভুক্ত হয়েছে; repo notes-এর নির্দেশ অনুযায়ী chapter status-এ শুধু Draft checked থাকবে, Technical Review/Security Review checkbox unchecked থাকবে। কোনো implementation/browser test বা production certification দাবি করা হচ্ছে না.
- নীরার email-এর cancel link শুধু order page দেখাবে (GET); order cancel ও logout state-changing POST action। Safe method-এ state change করা যাবে না।
- KenaKata-র Fetch Metadata policy: state-changing request-এ same-origin পরের validation স্তরে যাবে; cross-site, same-site, none এবং অচেনা value reject হবে। Sec-Fetch-Site অনুপস্থিত হলে server-configured target origin-এর সঙ্গে Origin হুবহু মেলাতে হবে; দুটোই অনুপস্থিত বা Origin: null হলে fail-safe reject। এটি KenaKata-র threat-model decision, universal browser requirement নয়। Non-browser client header জাল করতে পারে, তাই header authentication নয়।
- Session-bound synchronizer CSRF token server-side session-এ থাকে, session ID থেকে স্বাধীন random value হয় এবং state-changing request-এ যাচাই হয়। Token URL বা log-এ থাকবে না। Login-এর আগে anonymous pre-session token login CSRF কমায়; successful login-এ পুরোনো anonymous session invalid করে নতুন session ID ও token তৈরি করতে হবে।
- Framework-এর built-in CSRF protection-কে implementation-এর সম্ভাব্য starting point বলা হয়েছে, কিন্তু তার default behavior ও missing-header policy যাচাই না করে গ্রহণ করার পরামর্শ দেওয়া হয়নি।
- notes/glossary.md-এ CSRF, Synchronizer Token ও Fetch Metadata যোগ করা হয়েছে। notes/misconceptions.md-এ Chapter 6-এর ১১টি misconception যোগ করা হয়েছে। references/owasp.md-এ CSRF Cheat Sheet-এর chapter scope ৬ ও ১০ করা হয়েছে। references/browser-security.md-এ W3C Fetch Metadata, WHATWG Fetch, RFC 9110, RFC 7617, Go CrossOriginProtection ও GO-2025-3955 reference যোগ করা হয়েছে।
- Sources reviewed: OWASP CSRF Prevention Cheat Sheet; MDN CSRF and Fetch Metadata guidance; W3C Fetch Metadata Request Headers (21 September 2026 Working Draft, not a Recommendation); WHATWG Fetch forbidden request-header names; RFC 7617 §2.2; RFC 9110 §9.2.1; Go vulnerability report GO-2025-3955. Go advisory-টি নির্দিষ্টভাবে CrossOriginProtection.AddInsecureBypassPattern-এর Go 1.25.0 issue; Go 1.25.1-এ fixed, সব CrossOriginProtection use-এর generic vulnerability নয়।
- Browser credential behavior, browser compatibility matrix, proxy behavior, application implementation, automated tests বা production configuration পরীক্ষা করা হয়নি। এগুলো manual follow-up হিসেবে রাখা হয়েছে, source-based review-কে runtime verification হিসেবে দেখানো হবে না।
- Chapter 6-এর 06-repo-notes.md-এর সব actionable item chapter, shared glossary/misconception/reference docs অথবা এই decision/review record-এ resolve করা হয়েছে; notes file-টি delete করা হয়েছে। Chapter Published চিহ্নিত করা হয়নি.



## 2026-10-25 — Chapter 8: token অধ্যায়ের scope

- Chapter 8-এর কেন্দ্রীয় সমস্যা: KenaKata-র first-party mobile app API request-এ credential কীভাবে বহন করবে। পুরোনো server-rendered web app-এর cookie session থাকবে; mobile আসার কারণে সেটি সরানো হবে না।
- Chapter 8-এ access-token ও cookie-session model তুলনা করা হবে, কিন্তু mobile app cookie ব্যবহার করতে পারে না বা token নিজে থেকেই বেশি নিরাপদ — এমন absolute claim করা হবে না।
- Bearer-token example-এ Authorization header, HTTPS, token disclosure/replay risk এবং token validation বনাম resource authorization-এর পার্থক্য থাকবে। Token URL/query string-এ দেখানো হবে না।
- KenaKata-র first-party mobile login-কে OAuth flow বলা হবে না। OAuth/PKCE/OIDC অধ্যায় ১৬–২১-এ থাকবে; JWT অধ্যায় ৯, SPA token storage অধ্যায় ১০, expiry/replay অধ্যায় ১১, refresh-token lifecycle অধ্যায় ১২–১৪-এ থাকবে।
- RFC 6750 Chapter 8-এ bearer-token model বোঝাতে ব্যবহার হবে, তবে OAuth-specific semantics-কে KenaKata-র বর্তমান architecture-এর সত্য হিসেবে আগেভাগে প্রয়োগ করা হবে না।
- Chapter 8-এর scope ও source plan series/season-1-browser-session-token/08-tokens/08-repo-notes.md-এ আছে। এই এন্ট্রি সিদ্ধান্তের রেকর্ড; chapter draft বা review সম্পন্ন হওয়ার দাবি নয়.

## 2026-10-25 — Chapter 8-এর প্রথম Draft

- Chapter 8-এর first-party mobile access-token story, cookie-session comparison, illustrative Authorization: Bearer request, token validation বনাম authorization, bearer-token disclosure/replay risk, failure paths, exercises এবং RFC references যোগ করা হয়েছে।
- Chapter 8-এর Draft OAuth authorization-code flow, JWT internals, SPA token storage, expiry/replay timeline এবং refresh-token lifecycle আগেভাগে শেখায় না; এগুলো timeline অনুযায়ী পরের অধ্যায়ে থাকবে।
- Chapter 8 এখন Draft। Technical Review ও Security Review এখনও করা হয়নি। Mermaid source-এর basic fence/section check হয়েছে, কিন্তু Mermaid renderer বা browser-এ diagram render করে দেখা হয়নি।
- Source basis: RFC 6750-এর bearer-token definition, Authorization header, transport ও disclosure guidance; RFC 6749 শুধু OAuth-context access-token reference হিসেবে রাখা হয়েছে। RFC 6750-এর OAuth scope এবং KenaKata-র first-party login-এর মধ্যে পার্থক্য স্পষ্ট করা হয়েছে.

## 2026-10-25 — Chapter 8 Technical ও Security Review

- Chapter 8-এর source-based conceptual review করা হয়েছে। Password প্রতিটি request-এ পাঠানোর আলোচনায় HTTPS transport encryption-এর পার্থক্য স্পষ্ট করা হয়েছে; bearer token ব্যবহার করলেই নিরাপত্তা স্বয়ংক্রিয়ভাবে বাড়ে এমন দাবি রাখা হয়নি।
- Mermaid sequence diagram-এ missing/invalid token-এর 401 path এবং valid token-এর পরে authorization decision, permission না থাকলে 403 path আলাদা করা হয়েছে। Intended API/audience যাচাইয়ের কথাও স্পষ্ট করা হয়েছে।
- RFC 6750-এর bearer-token semantics, Authorization header, invalid-token/insufficient-scope behavior এবং token disclosure/TLS/URL risks source হিসেবে দেখা হয়েছে। KenaKata-র first-party login-কে OAuth flow বলা হয়নি।
- Review record: `notes/technical-review-chapter-8.md`। Chapter 8 Draft, source-based Technical Review ও Security Review হিসেবে চিহ্নিত; Published নয়। Mermaid renderer, browser বা mobile implementation test চালানো হয়নি.

## 2026-10-25 — Chapter 9-এর scope ও প্রথম Draft

- Chapter 9 JWT-কে token format হিসেবে শেখাবে, access token-এর সমার্থক হিসেবে নয়। Signed JWT/JWS-এর পরিচিত তিন অংশ, Base64URL encoding বনাম encryption, claims-এর সীমা, signature/integrity, এবং validation ও authorization-এর পার্থক্য ব্যাখ্যা করা হবে।
- তিন-part structure-কে সব JWT-এর সার্বজনীন গঠন বলা হবে না; JWE Compact Serialization-এ পাঁচটি অংশ থাকতে পারে। JWS signature payload গোপন করে না।
- Validation guidance-এ server-defined algorithm policy, trusted key, issuer, intended audience এবং প্রযোজ্য time claims থাকবে। এটি conceptual guidance, কোনো universal copy-paste validation recipe নয়।
- OAuth access-token profile, SPA storage, expiry/replay timeline ও refresh-token lifecycle পরের অধ্যায়ের scope-এই থাকবে।
- Source basis: RFC 7519 (JWT), RFC 7515 (JWS), RFC 8725 (JWT Best Current Practices)। Draft লেখার পর source-based Technical ও Security Review আলাদা করে করতে হবে; browser/app test বা production certification দাবি করা যাবে না।


## 2026-10-25 — Chapter 9 Technical ও Security Review

- Chapter 9-এর source-based conceptual review-এ JWT বনাম access token, JWS-এর signing input ও integrity protection, Base64URL encoding বনাম encryption, JWS/JWE serialization-এর পার্থক্য, trusted key/algorithm policy, issuer/audience/time claims এবং authorization boundary যাচাই করা হয়েছে।
- Mermaid signing-input diagram সংশোধন করা হয়েছে। JWT-সম্পর্কিত ভুল ধারণাগুলো `notes/misconceptions.md`-এ যোগ করা হয়েছে।
- Review record: `notes/technical-review-chapter-9.md`। Chapter 9-এর Technical Review ও Security Review source-based review হিসেবে checked; Published নয়।
- Mermaid renderer, browser, mobile বা application implementation test চালানো হয়নি। এটি production security audit বা certification নয়।


## 2026-10-25 — Chapter 10: SPA token storage

- Chapter 10-এ localStorage, sessionStorage, in-memory storage, HttpOnly cookie এবং Backend for Frontend (BFF) তুলনা করা হয়েছে; কোনো storage-কে সব threat-এর সর্বজনীন সমাধান বলা হয়নি।
- KenaKata-র product timeline অক্ষুণ্ণ: SPA browser client, mobile-এর access/refresh token আগে থেকেই আছে; এই অধ্যায়ে OAuth flow চালু করা হয়নি।
- Cookie-ভিত্তিক authentication-এ Chapter 6-এর KenaKata-specific CSRF policy বহাল। BFF token browser থেকে আড়াল করলেও XSS-এর মাধ্যমে authenticated action-এর ঝুঁকি রয়ে যায়।
- Source basis: RFC 10017 (August 2026 Internet Best Current Practice), OWASP HTML5 Security / Session Management / CSRF cheat sheets এবং MDN cookie/Web Storage documentation.
- Chapter 10-এর Technical ও Security Review source-based conceptual review; browser test, Mermaid renderer test বা production audit নয়। Review record: notes/technical-review-chapter-10.md.


- Chapter 10 follow-up review (2026-10-25): clarified that the illustrative cookie lacks Max-Age/Expires and therefore does not promise persistence across browser restarts; persistent login depends on cookie lifetime plus server-side session lifetime and server-side invalidation on logout. Clarified RFC 10017 §6.1 (OAuth BFF) vs §7.1 (single-domain non-OAuth context) so KenaKata is not implied to implement OAuth.

## 2026-10-25 — Chapter 11–14 এবং OAuth scope continuity

- Chapter 11-এর wording সংশোধন করা হয়েছে: refresh-token-এর generic lifecycle অধ্যায় ১২–১৪-তে; OAuth authorization flow ও protocol-specific নিয়ম অধ্যায় ১৬–২০-তে থাকবে।
- Chapter 12-এর scope হবে refresh token কেন দরকার, access token-এর সঙ্গে পার্থক্য, token renewal-এর conceptual flow, দীর্ঘমেয়াদি credential-এর ঝুঁকি, storage/binding/lifetime-এর baseline, এবং design trade-off।
- Chapter 13-তে rotation, token-family/reuse detection, race condition ও breach response-এর গভীর আলোচনা থাকবে। Chapter 14-তে logout, revocation এবং ইতিমধ্যে issue হওয়া access token-এর আচরণ থাকবে।
- OAuth-নির্দিষ্ট normative requirements শেখানোর সময় RFC 6749-এর পাশাপাশি RFC 9700-এর বর্তমান OAuth Security BCP ব্যবহার করতে হবে; generic first-party mobile flow-কে OAuth implementation বলা যাবে না।
- এই scope note Chapter 12-এর draft নয়; লেখার আগে product documents, আগের chapter, authoritative references এবং `CONTRIBUTING.md` আবার মিলিয়ে নিতে হবে।
