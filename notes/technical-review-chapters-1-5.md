# অধ্যায় ১–৫: Technical Review নোট

তারিখ: ২০২৬-১০-২৫  
Branch: `improvements/chapters-engagement`  
Pull request: [#1](https://github.com/shomi-khan/auth-security-journey/pull/1)

এই নোটটি review-এর অগ্রগতি ধরে রাখে। এটি security certification নয়। নিচের সংশোধনগুলো branch-এ করা হয়েছে, কিন্তু এই review-তে chapter-এর সব code snippet একত্র করে compile করা, automated test চালানো, বা বাস্তব browser-এ পরীক্ষা করা হয়নি। তাই পাঁচটি অধ্যায়ের `Technical Review` ও `Security Review` checkbox এখনো অসম্পূর্ণ থাকবে।

## এই review-তে করা সংশোধন

### Chapter 3: Password Security

- **[Medium] Argon2 encoded parameter-এর পুরো field যাচাই হচ্ছিল না।** `fmt.Sscanf` দিয়ে version ও parameter পড়ার পর এখন canonical string-এর সঙ্গে তুলনা করা হয়। এতে `v=19junk` বা parameter field-এ অতিরিক্ত অক্ষর থাকলে তা গ্রহণ করা হবে না।
- **[Medium] Dummy hash তৈরি করার error উপেক্ষা করা হচ্ছিল।** এখন initialization ব্যর্থ হলে application শুরুতেই fail করবে, যাতে অজান্তে nil/খালি dummy hash নিয়ে চলতে না থাকে।
- **Source verification:** RFC 9106-এর §3.1, §4 এবং §7.4 মূল উৎস থেকে মিলিয়ে reference note সংশোধন করা হয়েছে। RFC-এর 2 GiB / 64 MiB recommendation-কে OWASP-এর 19 MiB, `t=2`, `p=1` minimum configuration-এর সঙ্গে এক করে দেখা যাবে না। বাস্তব workload-এ benchmark জরুরি।

### Chapter 5: Session Hijacking এবং Session Fixation

- **[Medium] `__Secure-` prefix-এর ক্ষমতা অতিরঞ্জিত ছিল।** এখন স্পষ্ট করা হয়েছে যে এটি `Secure` attribute বাধ্যতামূলক করে, কিন্তু `Domain` নিষিদ্ধ করে না। `__Host-` prefix-এর `Secure`, no-`Domain`, `Path=/` শর্ত আলাদা করে ব্যাখ্যা করা হয়েছে। Session ID rotate করাই fixation প্রতিরোধের মূল নিয়ম; cookie integrity অতিরিক্ত প্রতিরক্ষা।

## Review-তে নিশ্চিত করা গুরুত্বপূর্ণ নীতি

- Chapter 1: গোপন URL authentication নয়; log-এর IP ও timestamp একা কোনো মানুষের পরিচয়ের প্রমাণ নয়।
- Chapter 2: Authentication, function-level authorization ও object-level authorization আলাদা সিদ্ধান্ত। Unit test-এ permission function যাচাই হওয়া route-গুলোর সম্পূর্ণ enforcement প্রমাণ করে না।
- Chapter 3: Password hash-এর algorithm ও parameter encoded string থেকে পড়লেও input-কে অবিশ্বাস্য ধরতে হবে। Online rate limit database dump হাতে পাওয়া আক্রমণকারীর offline guessing আটকায় না।
- Chapter 4: Cookie শুধু বাহক; server-side session record revocation সম্ভব করে। `HttpOnly` cookie পড়া কঠিন করে, কিন্তু browser-কে সেই cookie-সহ request পাঠানো থেকে আটকায় না। `SameSite=Lax` একা CSRF সমাধান নয়।
- Chapter 5: Idle timeout সক্রিয় attacker-কে থামায় না; absolute timeout ও revocation আলাদা ভূমিকা রাখে। IP/User-Agent risk signal, পরিচয়ের প্রমাণ নয়। Login-এ session ID rotate করতে হবে এবং পুরোনো ID অকার্যকর করতে হবে।

## বাকি যাচাই ও সীমাবদ্ধতা

1. **Chapter 1 ও 4-এর HTTP 401 উদাহরণ:** RFC 9110 অনুযায়ী 401 response-এ `WWW-Authenticate` challenge থাকা দরকার। Chapter 4 নিজেই স্বীকার করে যে cookie-session উদাহরণে challenge অনুপস্থিত। এটি একটি নথিভুক্ত standards caveat; এই review-তে কোনো ভুয়া বা বিভ্রান্তিকর challenge যোগ করা হয়নি। API contract স্থির করার সময় এটি আলাদা করে সমাধান করতে হবে।
2. **Chapter 2:** Permission function-এর unit test ছাড়াও প্রতিটি protected route-এ authentication ও function/object-level authorization প্রয়োগ হচ্ছে কি না, তার integration test দরকার।
3. **Chapter 3:** `norm.NFC` ও `checkPolicy`-র আচরণ, malformed PHC string-এর boundary case এবং parser-এর নতুন negative case বাস্তবে চালিয়ে যাচাই করা বাকি। RFC/OWASP parameter-এর উপযোগিতা production server-এ benchmark ছাড়া চূড়ান্ত বলা যাবে না।
4. **Chapter 4–5:** বাস্তব browser-এ cookie prefix, `SameSite`, HTTPS/HSTS, concurrent request ও session rotation আচরণ যাচাই করা বাকি। একাধিক application instance বা distributed session store-এ revocation consistency-ও এই review-তে পরীক্ষা করা হয়নি।
5. **সব অধ্যায়:** উদাহরণে কাল্পনিক handler/function ও সরলীকরণ আছে। এগুলোকে production-ready application হিসেবে ব্যবহার করা যাবে না। Security review শেষ বলতে হলে source claim, executable example/test, route coverage এবং browser/deployment সীমা আলাদা করে যাচাই করতে হবে।

## পরবর্তী ধাপ

- Chapter 3-এর malformed-hash test case যোগ করে parser-এর positive ও negative behavior চালানো।
- Chapter 1–5-এর code snippet-গুলোকে “compile করা যায়” বলে দাবি করার আগে প্রতিটি snippet-এর scope স্পষ্ট করা এবং সম্ভব হলে runnable companion test রাখা।
- Chapter 4-এর 401/API response design নিয়ে সিদ্ধান্ত নথিভুক্ত করা।
- বাকি যাচাই শেষ না হওয়া পর্যন্ত chapter status checkbox unchecked রাখা।
