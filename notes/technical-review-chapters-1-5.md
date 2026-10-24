# অধ্যায় ১–৫: Technical Review নোট

তারিখ: ২০২৬-১০-২৫  
Branch: `improvements/chapters-engagement`  
Pull request: [#1](https://github.com/shomi-khan/auth-security-journey/pull/1)

এই নোটটি review-এর অগ্রগতি ধরে রাখে। এটি security certification নয়। Chapter 3-এর current diagram-first draft-এর conceptual Technical Review ও Security Review সম্পন্ন হয়েছে: core claims authoritative sources-এর সঙ্গে মিলিয়ে দেখা হয়েছে, threat model, limitations, failure paths ও diagrams পর্যালোচনা করা হয়েছে। কোনো code test, parser audit, production deployment test বা browser test করা হয়নি। অন্য অধ্যায়ের review status আলাদা থাকবে।

## পূর্ববর্তী code-oriented draft-এ করা সংশোধন (historical)

### Chapter 3: Password Security

- **[Medium] Argon2 encoded parameter-এর পুরো field যাচাই হচ্ছিল না।** `fmt.Sscanf` দিয়ে version ও parameter পড়ার পর এখন canonical string-এর সঙ্গে তুলনা করা হয়। এতে `v=19junk` বা parameter field-এ অতিরিক্ত অক্ষর থাকলে তা গ্রহণ করা হবে না।
- **[Medium] Dummy hash তৈরি করার error উপেক্ষা করা হচ্ছিল।** এখন initialization ব্যর্থ হলে application শুরুতেই fail করবে, যাতে অজান্তে nil/খালি dummy hash নিয়ে চলতে না থাকে।
- **Unicode normalization:** password policy, Argon2id `Hash` এবং `Verify` উদাহরণে একই `norm.NFC.String` normalization যোগ করা হয়েছে। Registration/reset/login-এর সব পথেই একই নিয়ম বজায় রাখতে হবে। এই integration আলাদা করে চালিয়ে পরীক্ষা করা বাকি।
- **Source verification:** RFC 9106-এর §3.1, §4 এবং §7.4 মূল উৎস থেকে মিলিয়ে reference note সংশোধন করা হয়েছে। RFC-এর 2 GiB / 64 MiB recommendation-কে OWASP-এর 19 MiB, `t=2`, `p=1` minimum configuration-এর সঙ্গে এক করে দেখা যাবে না। বাস্তব workload-এ benchmark জরুরি।

### Chapter 5: Session Hijacking এবং Session Fixation

- **[Medium] `__Secure-` prefix-এর ক্ষমতা অতিরঞ্জিত ছিল।** এখন স্পষ্ট করা হয়েছে যে এটি `Secure` attribute বাধ্যতামূলক করে, কিন্তু `Domain` নিষিদ্ধ করে না। `__Host-` prefix-এর `Secure`, no-`Domain`, `Path=/` শর্ত আলাদা করে ব্যাখ্যা করা হয়েছে। Session ID rotate করাই fixation প্রতিরোধের মূল নিয়ম; cookie integrity অতিরিক্ত প্রতিরক্ষা।
- **Password-change flow-তে session rotation error উপেক্ষা করা হচ্ছিল।** উদাহরণে cookie অনুপস্থিত থাকলে এখন `401`, আর rotation ব্যর্থ হলে `500` ফেরত দেওয়া হয়। Rotation সফল হওয়ার পর নতুন session-টি রেখে বাকি session বাতিল করা হয়। এটি documentation-level code correction; runnable test-এ এখনো পুনরায় যাচাই করা হয়নি।
- **Expired anonymous session-এর state বহন হওয়ার ঝুঁকি ছিল।** `Rotate` এখন পুরোনো session-এর `Next` কেবল সেটি তখনও সক্রিয় এবং anonymous হলে বহন করে; expired row থাকলে সেটি মুছে ফেলে। নতুন code-এ regression test যোগ করা ও চালানো বাকি।

## Review-তে নিশ্চিত করা গুরুত্বপূর্ণ নীতি

- Chapter 1: গোপন URL authentication নয়; log-এর IP ও timestamp একা কোনো মানুষের পরিচয়ের প্রমাণ নয়।
- Chapter 2: Authentication, function-level authorization ও object-level authorization আলাদা সিদ্ধান্ত। Unit test-এ permission function যাচাই হওয়া route-গুলোর সম্পূর্ণ enforcement প্রমাণ করে না।
- Chapter 3: Password hash-এর algorithm ও parameter encoded string থেকে পড়লেও input-কে অবিশ্বাস্য ধরতে হবে। Online rate limit database dump হাতে পাওয়া আক্রমণকারীর offline guessing আটকায় না।
- Chapter 4: Cookie শুধু বাহক; server-side session record revocation সম্ভব করে। `HttpOnly` cookie পড়া কঠিন করে, কিন্তু browser-কে সেই cookie-সহ request পাঠানো থেকে আটকায় না। `SameSite=Lax` একা CSRF সমাধান নয়।
- Chapter 5: Idle timeout সক্রিয় attacker-কে থামায় না; absolute timeout ও revocation আলাদা ভূমিকা রাখে। IP/User-Agent risk signal, পরিচয়ের প্রমাণ নয়। Login-এ session ID rotate করতে হবে এবং পুরোনো ID অকার্যকর করতে হবে।

- **Chapter 2 route method:** Refund handler-এর উদাহরণে এখন method-specific `POST /manage/orders/{id}/refund` route registration স্পষ্ট করা হয়েছে। `POST` ব্যবহার করলেও CSRF প্রতিরক্ষা আলাদা, এবং route-level integration test এখনো দরকার।

## বাকি যাচাই ও সীমাবদ্ধতা

1. **Chapter 1 ও 4-এর HTTP 401 উদাহরণ:** RFC 9110 অনুযায়ী 401 response-এ `WWW-Authenticate` challenge থাকা দরকার। Chapter 4 নিজেই স্বীকার করে যে cookie-session উদাহরণে challenge অনুপস্থিত। এটি একটি নথিভুক্ত standards caveat; এই review-তে কোনো ভুয়া বা বিভ্রান্তিকর challenge যোগ করা হয়নি। API contract স্থির করার সময় এটি আলাদা করে সমাধান করতে হবে।
2. **Chapter 2:** Permission function-এর unit test ছাড়াও প্রতিটি protected route-এ authentication ও function/object-level authorization প্রয়োগ হচ্ছে কি না, তার integration test দরকার।
3. **Chapter 3:** পুরোনো implementation-code revision-এর `norm.NFC`, `checkPolicy`, malformed PHC string parser ও test-সংক্রান্ত findings বর্তমান diagram-first chapter-এর review criteria নয়। বর্তমান review-তে password hashing, salt, offline guessing, login enumeration, password policy, Unicode handling-এর মূল ধারণা ও সীমাবদ্ধতা এবং authoritative source-এ করা claims যাচাই করতে হবে। এই সিরিজের scope-এ parser implementation audit বা production parameter tuning নেই।
4. **Chapter 4–5:** বাস্তব browser-এ cookie prefix, `SameSite`, HTTPS/HSTS, concurrent request ও session rotation আচরণ যাচাই করা বাকি। একাধিক application instance বা distributed session store-এ revocation consistency-ও এই review-তে পরীক্ষা করা হয়নি।
5. **সব অধ্যায়:** উদাহরণে কাল্পনিক handler/function ও সরলীকরণ আছে। এগুলোকে production-ready application হিসেবে ব্যবহার করা যাবে না। Security review শেষ বলতে হলে source claim, executable example/test, route coverage এবং browser/deployment সীমা আলাদা করে যাচাই করতে হবে।

## পরবর্তী ধাপ

- Chapter 3-এর current diagram-first draft-এর conceptual review সম্পন্ন; parser implementation audit এই series-এর scope নয়।
- Chapter 1–5-এর code snippet-গুলোকে “compile করা যায়” বলে দাবি করার আগে প্রতিটি snippet-এর scope স্পষ্ট করা এবং সম্ভব হলে runnable companion test রাখা।
- Chapter 4-এর 401/API response design নিয়ে সিদ্ধান্ত নথিভুক্ত করা।
- Chapter 1–2 ও 4–5-এর বাকি যাচাই সম্পন্ন না হওয়া পর্যন্ত সেগুলোর review checkbox unchecked রাখা।


## 2026-10-25 — Chapter 3 diagram-first migration note

The Chapter 3 bullets above that mention Go, `norm.NFC`, `checkPolicy`, parser tests, and prior compile/test history describe an earlier implementation-code revision. The current Chapter 3 teaching draft has been converted to Mermaid diagrams and language-neutral examples; those implementation-specific findings are historical and should not be read as claims about the current chapter content.

The current draft has not been granted Technical Review or Security Review sign-off. Future review should focus on password-storage concepts, account-enumeration mitigations, online/offline threat boundaries, meaningful limitations and trade-offs, and the accuracy of diagrams and source references. Tool-specific parameter parsing and PHC-format validation are outside the teaching draft's scope unless needed to explain a core security property. No code tests were run for this diagram-first update.


## 2026-10-25 — Review scope updated to theory-first

Chapter 3 has been simplified to emphasize the security concepts: why passwords should not be stored in plaintext or reversible encryption, why fast general-purpose hashes are unsuitable, how unique salts help, why password hashing must be intentionally expensive, why password strength still matters, how online and offline attacks differ, and what login enumeration mitigations can and cannot do.

The former implementation-specific notes about Argon2id parameter parsing, PHC string parsing, and code-level Unicode normalization were written for an earlier code-oriented draft. They are historical context, not mandatory implementation work for the current teaching draft. Review should verify conceptual accuracy, relevant threat boundaries, meaningful failure paths, and source-backed claims. It should not turn the chapter into an implementation manual or imply production security certification.

Chapter 3's Technical Review and Security Review checkboxes are now checked for conceptual, source-based review only. No code tests, parser implementation audit, production parameter tuning, load benchmark, or deployment test were run.


## 2026-10-25 — Chapter 4 theory-first rewrite

Chapter 4's Go snippets, executable shell command, and code-specific test history were removed from the teaching chapter. The replacement focuses on the browser/server/session mental model, cookie attributes, server-side expiry and revocation, session-store exposure, and the limits of each control. Mermaid diagrams and language-neutral HTTP examples are now the primary explanatory tools.

Chapter 4 Technical Review and Security Review remain unchecked pending a focused source-based review of the revised chapter's conceptual claims, cookie behavior, session lifecycle, and threat boundaries. No implementation tests or browser tests were run for this revision.


## 2026-10-25 — Chapter 4 conceptual review completed

- [x] Session ID as a bearer credential, server-side session lookup, expiry and revocation checked against OWASP Session Management guidance and RFC 6265.
- [x] Cookie attributes reviewed: `Secure`, `HttpOnly`, `SameSite`, `Path`, `Domain`, and `__Host-`; the chapter states both what each helps with and what it does not solve.
- [x] Session ID entropy, meaningless identifiers, strict acceptance of server-issued IDs, and the distinction between random session ID verifiers and password hashing checked against OWASP guidance.
- [x] Browser-cookie expiry versus server-side session expiry, logout/replay behavior, and session-store read-only disclosure threat boundary reviewed.
- [x] Removed code-specific claims and clarified that a stolen session can be as serious as password theft within the application, while typically not reusable across unrelated sites.
- [x] Refreshed the citation note for draft-ietf-httpbis-rfc6265bis-22: as of 2026-10-25, its RFC Editor state is blocked pending final approval, so it must still be described as a draft, not a published RFC.

The Chapter 4 Technical Review and Security Review checkboxes are checked for conceptual, source-based review only. No code tests, live browser compatibility tests, production deployment tests, or exhaustive implementation audit were performed. Chapter 5 and the other remaining review items are still separate work.
