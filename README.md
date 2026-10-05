# Auth Security Journey

বাংলাভাষী Software Engineer-দের জন্য Authentication, Authorization এবং Web Security শেখার একটি গল্পভিত্তিক যাত্রা।

## এই repository কী?

এটা ২৫ অধ্যায়ের বাংলা technical series।

গল্পে একটা fictional product আছে, নাম **KenaKata**। ছোট একটা দোকানের অ্যাপ থেকে সেটা ধীরে ধীরে SaaS platform হয়। সেই পথ ধরে Authentication, Authorization, আর Web/API Security শেখা হবে।

এখনো কোনো অধ্যায় লেখা হয়নি। নিচের তালিকায় সব জায়গায় “লেখা হয়নি” আছে। অধ্যায় প্রকাশিত হলে তবেই সেটা বদলাবে।

KenaKata সম্পর্কে যে তথ্য পুরো সিরিজে মিলিয়ে চলতে হবে, সেটা এখানে: [product/](product/README.md)।

## কার জন্য?

এই সিরিজ বাংলাভাষী junior software engineer-দের জন্য।

বিশেষ করে:

- Junior Software Engineer
- Backend Developer
- Frontend Developer
- যারা Web Security শিখতে চান
- যারা Authentication, OAuth, JWT দেখে confused হন

তুমি programming-এর বেসিক জানো। HTTP আর REST API চেনো। Backend বা frontend-এ কিছু কাজ করেছ। Authentication, Authorization, OAuth, OIDC, JWT, Session, CSRF, XSS আলাদা আলাদা শুনেছ। একটা আরেকটার সঙ্গে কীভাবে জোড়া লাগে, সেটা এখনো পরিষ্কার নয়।

তোমাকে নতুন করে programming শেখানো হবে না। আবার বিষয়গুলোকে ছোট করে এমন সরল করা হবে না, যেটা পরে কাজে লাগে না।

## কীভাবে শেখানো হবে?

প্রতিটা অধ্যায় KenaKata-র একটা সমস্যা দিয়ে শুরু হয়।

```text
সমস্যা
  ↓
সহজ সমাধান
  ↓
সমাধান ভেঙে গেল
  ↓
Security Problem
  ↓
ভালো সমাধান
  ↓
Trade-off
```

আরিফ প্রথমে যেটা সহজ মনে হয় সেটা বানায়। সেটা ভাঙে। তখন আসল সমস্যা দেখা যায়। তারপর একটা ভালো সমাধান, আর সেই সমাধানের দাম। নতুন জিনিসটাকে শেষ কথা বানানো হবে না।

## Learning Goal

> লক্ষ্য শুধু JWT, OAuth বা Session-এর definition মুখস্থ করানো নয়। লক্ষ্য হলো এমন একটি mental model তৈরি করা, যাতে একজন junior engineer নতুন application-এর authentication এবং authorization architecture নিয়ে নিজে reasoning করতে পারে।

## Season 1

### Browser, Session এবং Token

অধ্যায় ১–১৫। [Season 1](series/season-1-browser-session-token/README.md)

এই season-এ শেখা হবে:

- Authentication
- Authorization
- Password
- Session
- Cookie
- CSRF
- XSS
- Token
- JWT
- Access Token
- Refresh Token
- Revocation
- IDOR/BOLA

## Season 2

### OAuth, OIDC এবং Delegated Identity

অধ্যায় ১৬–২১। [Season 2](series/season-2-oauth-oidc/README.md)

এই season-এ শেখা হবে:

- OAuth
- Authorization Code
- state
- redirect URI
- PKCE
- OIDC
- ID Token
- Access Token
- Google Login

## Season 3

### Production Security

অধ্যায় ২২–২৫। [Season 3](series/season-3-production-security/README.md)

এই season-এ শেখা হবে:

- MFA
- Step-up Authentication
- Machine-to-Machine Authentication
- Architecture decision
- Complete production architecture

## অধ্যায়সূচি

`লেখা হয়নি` মানে অধ্যায়ের আর্টিকেল এখনো নেই। নামের উপর ক্লিক করলে placeholder-এ যাবে।

### Season 1 — Browser, Session এবং Token

## [অধ্যায় ১ — কেন Authentication দরকার?](series/season-1-browser-session-token/01-why-authentication/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ২ — Authentication বনাম Authorization](series/season-1-browser-session-token/02-authentication-vs-authorization/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৩ — প্রথম Login System এবং Password Security](series/season-1-browser-session-token/03-password-security/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৪ — Session এবং Cookie](series/season-1-browser-session-token/04-session-and-cookie/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৫ — Session Hijacking এবং Session Fixation](series/season-1-browser-session-token/05-session-hijacking-fixation/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৬ — CSRF](series/season-1-browser-session-token/06-csrf/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৭ — XSS এবং HttpOnly-এর সীমাবদ্ধতা](series/season-1-browser-session-token/07-xss-and-httponly/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৮ — Token কেন দরকার?](series/season-1-browser-session-token/08-tokens/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ৯ — JWT আসলে কী?](series/season-1-browser-session-token/09-jwt/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১০ — SPA-তে Token কোথায় রাখব?](series/season-1-browser-session-token/10-spa-token-storage/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১১ — Token Expiration এবং Replay Window](series/season-1-browser-session-token/11-token-expiration-replay/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১২ — Refresh Token কেন?](series/season-1-browser-session-token/12-refresh-token/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১৩ — Refresh Token Rotation এবং Reuse Detection](series/season-1-browser-session-token/13-refresh-token-rotation/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১৪ — Logout এবং Revocation](series/season-1-browser-session-token/14-logout-revocation/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১৫ — IDOR/BOLA এবং Authorization Design](series/season-1-browser-session-token/15-idor-bola-authorization/README.md)

- [ ] লেখা হয়নি

### Season 2 — OAuth, OIDC এবং Delegated Identity

## [অধ্যায় ১৬ — OAuth 2.0 কেন?](series/season-2-oauth-oidc/16-oauth/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১৭ — Authorization Code Flow এবং `state`](series/season-2-oauth-oidc/17-authorization-code-state/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১৮ — Redirect URI এবং Authorization Code Theft](series/season-2-oauth-oidc/18-redirect-uri-code-theft/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ১৯ — PKCE](series/season-2-oauth-oidc/19-pkce/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ২০ — OIDC](series/season-2-oauth-oidc/20-oidc/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ২১ — Google Login এবং ID Token বনাম Access Token](series/season-2-oauth-oidc/21-google-login/README.md)

- [ ] লেখা হয়নি

### Season 3 — Production Security

## [অধ্যায় ২২ — MFA এবং Step-up Authentication](series/season-3-production-security/22-mfa-step-up/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ২৩ — Machine-to-Machine Authentication](series/season-3-production-security/23-machine-to-machine/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ২৪ — Context বদলালে Architecture কেন বদলায়?](series/season-3-production-security/24-context-and-architecture/README.md)

- [ ] লেখা হয়নি

## [অধ্যায় ২৫ — Capstone: পুরো KenaKata Security Architecture](series/season-3-production-security/25-capstone/README.md)

- [ ] লেখা হয়নি

## বাকি ডিরেক্টরি

| জায়গা | কী আছে |
| --- | --- |
| [product/](product/README.md) | KenaKata-র তথ্য। Chapter লেখার আগে এটা পড়তে হবে |
| [references/](references/README.md) | Spec আর cheat sheet-এর তালিকা। দাবি লেখার আগে এখান থেকে মিলিয়ে নিতে হবে |
| [notes/](notes/glossary.md) | Glossary, ভুল ধারণা, আর সিদ্ধান্তের খাতা |
| [diagrams/](diagrams/authentication/) | আঁকা ছবির জায়গা। এখন খালি। দরকার হলে chapter-এর সঙ্গে যোগ হবে |
| [examples/go/](examples/go/authentication/) | ছোট Go উদাহরণের জায়গা। এখন খালি। পুরো backend এখানে বানানো হবে না |

লেখার নিয়ম: [CONTRIBUTING.md](CONTRIBUTING.md)।

লাইসেন্স MIT। Copyright-এর নাম এখনো বসানো হয়নি, ফাইলে `[COPYRIGHT HOLDER]` লেখা আছে। দেখো [LICENSE](LICENSE)।
