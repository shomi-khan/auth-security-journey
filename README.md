# Trust Stack

**বাংলায় Authentication, Authorization এবং Web Security শেখার গল্পভিত্তিক সিরিজ।**

একটা ছোট দোকানের অ্যাপ কীভাবে ধীরে ধীরে SaaS platform হয়, আর সেই পথে security-র কোন সমস্যাগুলো সামনে আসে? এই সিরিজে আমরা আরিফের সঙ্গে **KenaKata** তৈরি করতে করতে সেই প্রশ্নগুলোর উত্তর খুঁজি।

> **শেখার লক্ষ্য:** শুধু Session, JWT বা OAuth-এর সংজ্ঞা মুখস্থ করা নয়। নতুন application-এর authentication ও authorization architecture নিয়ে নিজে reasoning করতে শেখা।

## এই প্রজেক্টে কী আছে?

- ২৬ অধ্যায়ের পরিকল্পিত বাংলা technical series
- Browser session ও cookie থেকে শুরু করে token, OAuth/OIDC এবং production security পর্যন্ত ধাপে ধাপে শেখা
- গল্প, threat model, diagram এবং trade-off-ভিত্তিক ব্যাখ্যা
- Security-সংক্রান্ত দাবির জন্য RFC, specification ও authoritative reference
- একই fictional product universe, যাতে অধ্যায়গুলোর উদাহরণ পরস্পরের সঙ্গে সামঞ্জস্যপূর্ণ থাকে

### বর্তমান অগ্রগতি

| অংশ | অবস্থা |
| --- | --- |
| অধ্যায় ১–১৬ | পূর্ণাঙ্গ draft আছে; cross-chapter consistency ও clarity review চলছে |
| অধ্যায় ৭: CORS | Technical ও security review সম্পন্ন |
| অধ্যায় ৯–১৬ | Draft article আছে; chapter-এ থাকা review notes ও scope আলাদাভাবে দেখো |
| অধ্যায় ১৩–১৬ | পূর্ণাঙ্গ draft আছে; Published নয় |
| অধ্যায় ১৭–২৬ | Chapter plan/brief পর্যায়ে; পূর্ণাঙ্গ article নয় |

**Status-এর অর্থ:** কোনো অধ্যায়ের draft বা brief থাকা মানেই সেটি published নয়। Review সম্পন্ন হওয়াও publication status-এর সমান নয়।

## কার জন্য?

বাংলাভাষী junior software engineer-দের জন্য, যারা programming-এর বেসিক জানেন এবং HTTP ও REST API-এর সঙ্গে পরিচিত। Authentication, Authorization, Session, Cookie, JWT, OAuth, OIDC, CSRF, CORS ও XSS-এর নাম শুনেছেন, কিন্তু ধারণাগুলো কীভাবে একে অন্যের সঙ্গে সম্পর্কিত, সেটা পরিষ্কার করতে চান।

এটি programming-এর প্রাথমিক কোর্স নয়, আবার কোনো নির্দিষ্ট framework বা security library-র implementation manual-ও নয়।

## শেখানোর পদ্ধতি

প্রতিটি অধ্যায় KenaKata-র একটি বাস্তবসম্মত সমস্যা দিয়ে শুরু হয়। সহজ সমাধানটি কোথায় ব্যর্থ হয়, সেই failure কী security risk তৈরি করে, এবং বিকল্প সমাধানের সুবিধা ও সীমাবদ্ধতা কী, তা ধাপে ধাপে দেখা হয়।

```text
সমস্যা
  ↓
সহজ সমাধান
  ↓
সমাধানের ব্যর্থতা
  ↓
Security risk
  ↓
উন্নত design
  ↓
Trade-off ও সীমাবদ্ধতা
```

মূল ফোকাস হলো security mental model, threat model, trust boundary এবং design trade-off। উদাহরণ ও diagram ধারণা পরিষ্কার করতে সাহায্য করবে; কোনো নির্দিষ্ট ভাষার runnable implementation এই সিরিজের লক্ষ্য নয়।

## অধ্যায়সূচি

### Season 1: Browser, Session এবং Token

| # | অধ্যায় | অগ্রগতি |
| --- | --- | --- |
| 1 | [কেন Authentication দরকার?](series/season-1-browser-session-token/01-why-authentication/README.md) | Draft |
| 2 | [Authentication বনাম Authorization](series/season-1-browser-session-token/02-authentication-vs-authorization/README.md) | Draft |
| 3 | [প্রথম Login System এবং Password Security](series/season-1-browser-session-token/03-password-security/README.md) | Draft |
| 4 | [Session এবং Cookie](series/season-1-browser-session-token/04-session-and-cookie/README.md) | Draft |
| 5 | [Session Hijacking এবং Session Fixation](series/season-1-browser-session-token/05-session-hijacking-fixation/README.md) | Draft |
| 6 | [CSRF](series/season-1-browser-session-token/06-csrf/README.md) | Draft |
| 7 | [CORS: Browser কোন Response পড়তে পারবে?](series/season-1-browser-session-token/07-cors/README.md) | Technical ও security review সম্পন্ন |
| 8 | [XSS এবং HttpOnly-এর সীমাবদ্ধতা](series/season-1-browser-session-token/08-xss-and-httponly/README.md) | Draft |
| 9 | [Token কেন দরকার?](series/season-1-browser-session-token/09-tokens/README.md) | Draft; review notes আছে |
| 10 | [JWT আসলে কী?](series/season-1-browser-session-token/10-jwt/README.md) | Draft |
| 11 | [SPA-তে Token কোথায় রাখব?](series/season-1-browser-session-token/11-spa-token-storage/README.md) | Draft |
| 12 | [Token Expiration এবং Replay Window](series/season-1-browser-session-token/12-token-expiration-replay/README.md) | Draft |
| 13 | [Refresh Token কেন?](series/season-1-browser-session-token/13-refresh-token/README.md) | Draft |
| 14 | [Refresh Token Rotation এবং Reuse Detection](series/season-1-browser-session-token/14-refresh-token-rotation/README.md) | Draft |
| 15 | [Logout, Revocation এবং Session Recovery](series/season-1-browser-session-token/15-logout-revocation/README.md) | Draft |
| 16 | [IDOR/BOLA এবং Authorization Design](series/season-1-browser-session-token/16-idor-bola-authorization/README.md) | Draft |

### Season 2: OAuth, OIDC এবং Delegated Identity

| # | অধ্যায় | অগ্রগতি |
| --- | --- | --- |
| 17 | [OAuth 2.0 কেন?](series/season-2-oauth-oidc/17-oauth/README.md) | Planned |
| 18 | [Authorization Code Flow এবং `state`](series/season-2-oauth-oidc/18-authorization-code-state/README.md) | Planned |
| 19 | [Redirect URI এবং Authorization Code Theft](series/season-2-oauth-oidc/19-redirect-uri-code-theft/README.md) | Planned |
| 20 | [PKCE](series/season-2-oauth-oidc/20-pkce/README.md) | Planned |
| 21 | [OIDC: OAuth-এর ওপর Identity Layer](series/season-2-oauth-oidc/21-oidc/README.md) | Planned |
| 22 | [Google Login এবং Token Validation](series/season-2-oauth-oidc/22-google-login/README.md) | Planned |

### Season 3: Production Security

| # | অধ্যায় | অগ্রগতি |
| --- | --- | --- |
| 23 | [MFA এবং Step-up Authentication](series/season-3-production-security/23-mfa-step-up/README.md) | Planned |
| 24 | [Machine-to-Machine Authentication](series/season-3-production-security/24-machine-to-machine/README.md) | Planned |
| 25 | [Context বদলালে Architecture কেন বদলায়?](series/season-3-production-security/25-context-and-architecture/README.md) | Planned |
| 26 | [Capstone: পুরো KenaKata Security Architecture](series/season-3-production-security/26-capstone/README.md) | Planned |

## Repository guide

| জায়গা | কী আছে |
| --- | --- |
| [`product/`](product/README.md) | KenaKata-র canonical product details, characters, timeline ও architecture |
| [`references/`](references/README.md) | RFC, specification এবং security reference-এর তালিকা |
| [`notes/glossary.md`](notes/glossary.md) | Security terminology-র glossary |
| [`notes/misconceptions.md`](notes/misconceptions.md) | সিরিজে যাচাই ও ভাঙার জন্য রাখা প্রচলিত ভুল ধারণা |
| [`notes/decision-log.md`](notes/decision-log.md) | গুরুত্বপূর্ণ editorial ও product সিদ্ধান্ত |
| [`notes/editorial-review-chapters-01-16.md`](notes/editorial-review-chapters-01-16.md) | অধ্যায় ১–১৬-এর cross-chapter consistency review ও তার সীমা |
| [`docs/code-example-policy.md`](docs/code-example-policy.md) | Code ও diagram ব্যবহারের নীতি |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | অধ্যায় লেখার ও review করার workflow |

KenaKata-র গল্পে কোনো feature কখন যুক্ত হয়েছে, তা যাচাই করতে [product timeline](product/timeline.md) দেখো। সিরিজের শেষ architecture-কে প্রথম অধ্যায় থেকেই চালু ধরে নেওয়া যাবে না।

## লেখার ও review-এর নীতি

- Security-সংক্রান্ত গুরুত্বপূর্ণ দাবির ক্ষেত্রে সংশ্লিষ্ট বর্তমান RFC, specification বা authoritative guidance যাচাই করতে হবে।
- Browser behavior বা configuration-এর উদাহরণকে বাস্তবে পরীক্ষা না করে test result হিসেবে উপস্থাপন করা যাবে না।
- Trade-off ও control-এর সীমাবদ্ধতা ব্যাখ্যা করতে হবে; কোনো technology-কে নিজে থেকেই নিরাপদ ধরে নেওয়া যাবে না।
- নতুন অধ্যায় লেখার আগে KenaKata-র canonical documents এবং [contribution guide](CONTRIBUTING.md) পড়তে হবে।

## License

এই repository MIT License-এর অধীনে প্রকাশিত। বিস্তারিত: [LICENSE](LICENSE)।
