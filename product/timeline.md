# Timeline

KenaKata কীভাবে এগোয়, এই ফাইল সেটা। উদ্দেশ্য একটাই: কোন অধ্যায়ে কোন feature আগে থেকে আছে, সেটা যেন না ভাঙে।

অধ্যায়ের ক্রম শেখানোর ক্রম। “এই জিনিসটা কি product-এ ইতিমধ্যে আছে?” — এই প্রশ্নে এই ফাইল জিতবে।

দুটো জায়গায় শেখানোর ক্রম আর product-এর ক্রম এক নয়। Product-এর ক্রম বদলায় না।

- Refresh token product-এ mobile app-এর সঙ্গে আসে। বিস্তারিত ব্যাখ্যা অধ্যায় ১৩–১৫-এ, SPA-র অধ্যায়ের পরে। SPA আসার সময় refresh token আগে থেকেই আছে। অধ্যায় ১১ বলতে পারে mobile client-এর কাছে এটা আছে। Rotation আর revocation সেখানে ব্যাখ্যা করবে না।
- অধ্যায় ১৬–২০ OAuth, PKCE, আর OIDC শেখায়। তখন Google login আর বাইরের app-এর connection এখনো দাবি, চালু feature নয়। Google login চালু হয় অধ্যায় ২১-এ। বাইরের application যুক্ত হয় তার পরে, Season 2-এর শেষে।

```text
Simple E-commerce
        ↓
Email + Password
        ↓
Session + Cookie
        ↓
Security Incident
        ↓
Mobile App
        ↓
Access Token + Refresh Token
        ↓
SPA
        ↓
Organization + Authorization
        ↓
Google Login
        ↓
Third-party Integration
        ↓
MFA
        ↓
Microservices
        ↓
Production Security Architecture
```

## Simple E-commerce

একটা server-rendered দোকান। Product, customer record, আর order। রাফির পোশাকের দোকান। Login-এর কোনো ডিজাইন এখনো নেই।

## Email + Password

দোকানের লোকজন email আর password দিয়ে ঢোকে। তখনো একটাই দোকান, অনেক tenant নয়।

## Session + Cookie

Server একটা session রাখে। Browser সেই session-এর পরিচয় cookie-তে নিয়ে চলে। এটাই প্রথম website-এর login।

## Security Incident

ওই cookie session-এ আক্রমণ হয়। Hijacking, fixation, CSRF, আর XSS এই ধাপের ভাঙন। নতুন product নয়। Architecture তখনো একটা web application।

## Mobile App

মালিক আর কর্মীদের জন্য mobile client আসে। Browser cookie আর একমাত্র রাস্তা থাকে না।

## Access Token + Refresh Token

Mobile client access token আর refresh token ব্যবহার করে। JWT তখন access token-এর format হিসেবে আলোচনায় আসতে পারে। পুরনো website তার cookie session রাখতে পারে।

## SPA

আসল website-এর পাশে একটা single-page application আসে। Token browser-এ কোথায় থাকবে, সেটা product-এর সমস্যা হয়। Refresh token আগে থেকেই আছে, কারণ mobile আগে এসেছে।

## Organization + Authorization

এক দোকান একটা organization হয়। অন্য ব্যবসাও যোগ দিতে পারে। Membership আর Owner, Admin, Member, Viewer role আসে। Product, customer, আর order একটা organization-এর। তাই “login করা আছে” আর যথেষ্ট নয়।

রাফির পোশাকের দোকান এখন একটা organization।

## Google Login

User Google দিয়ে login করতে পারে। `ExternalIdentity` আসে। KenaKata relying party। Google identity provider। বাইরের application তখনো চালু feature নয়।

## Third-party Integration

Organization বাইরের একটা application connect করতে পারে। `OAuthClient` আসে। ওই app-এর কাছে KenaKata authorization server। Google login-এর উল্টো দিক।

## MFA

সংবেদনশীল কাজে শুধু আগের login যথেষ্ট থাকে না। MFA আর step-up authentication product-এ আসে। কোন উপায়ে দ্বিতীয় ধাপ হবে, সেটা এখানে ধরা নেই। সংবেদনশীল কাজের তালিকা [product.md](product.md)-এ।

## Microservices

Backend আর একটা process নয়। User, order, billing, আর notification service API gateway-এর পিছনে। Service একে অপরকে চেনায়। Product catalog-এর আলাদা service এই সিরিজে নেই।

## Production Security Architecture

উপরের টুকরোগুলো এক system: client, gateway, service, authorization, Google, OAuth client, আর সংবেদনশীল কাজে step-up। ছবিটা [architecture.md](architecture.md)। শুরুর অধ্যায়ের architecture এটা নয়।

## কোন অধ্যায় কী ধরে নিতে পারে

| অধ্যায় | যা ইতিমধ্যে আছে |
| --- | --- |
| ১ | শুধু দোকান। Authentication সমস্যা, সমাধান এখনো নয় |
| ২ | সেই শুরুর দোকান। Authorization আলাদা প্রশ্ন। Organization এখনো নেই |
| ৩ | Email আর password আছে |
| ৪ | Session আর cookie আছে |
| ৫–৭ | Cookie session আক্রান্ত। Token তখনকার ডিজাইন নয় |
| ৮–৯ | Mobile app আছে। Access token আলোচনায়। JWT format হিসেবে আসতে পারে |
| ১০ | SPA আছে। Mobile-এর refresh token আগে থেকে থাকতে পারে |
| ১১–১৪ | মেয়াদ, refresh, rotation, logout, revocation |
| ১৫ | Organization, membership, role, আর resource কার সেটা |
| ১৬–২০ | Delegation শেখা হচ্ছে। Google login চালু নয়। বাইরের app-এর integration শেষ নয় |
| ২১ | Google login চালু |
| Season 2-এর শেষ | অধ্যায় ২১-এর পরে third-party integration চালু |
| ২২ | MFA আর step-up চালু |
| ২৩ | Microservice আর machine-to-machine authentication চালু |
| ২৪–২৫ | শেষ architecture বর্তমান ছবি হিসেবে আলোচনা করা যায় |


## Chapter plan status (২০২৬-১০-২৫)

অধ্যায় ১–৬-এর content এই পরিবর্তনে rewrite করা হয়নি। CORS-এর নতুন অধ্যায় ৭ যোগ হওয়ায় পুরোনো অধ্যায় ৭–২৫-এর নম্বর এক ধাপ এগিয়েছে। পুরোনো অধ্যায় ১১-এর draft এখন অধ্যায় ১২; অধ্যায় ১৩–২৬-এর README-তে পূর্ণাঙ্গ content লেখার আগে ব্যবহারযোগ্য brief থাকবে।

| অধ্যায় | পরিকল্পিত শিরোনাম | Brief-এর মূল ফোকাস |
| --- | --- | --- |
| ৭ | CORS: Browser কোন Response পড়তে পারবে? | Origin, same-origin policy, preflight, credentials এবং CORS-এর সীমা |
| ১২ | Token Expiration এবং Replay Window | Token lifetime, replay window ও layered control |
| ১৩ | Refresh Token কেন? | Refresh lifecycle ও credential-এর গুরুত্ব |
| ১৪ | Refresh Token Rotation এবং Reuse Detection | Rotation, reuse signal, race ও timeout recovery |
| ১৫ | Logout, Revocation এবং Session Recovery | Local logout, server revocation ও incident recovery |
| ১৬ | IDOR/BOLA এবং Authorization Design | Object-level ও tenant-level authorization |
| ১৭ | OAuth 2.0 কেন? | Delegated authorization ও OAuth-এর সীমানা |
| ১৮ | Authorization Code Flow এবং state | Flow sequence ও request/response correlation |
| ১৯ | Redirect URI এবং Authorization Code Theft | Redirect validation ও code interception |
| ২০ | PKCE | Code verifier/challenge ও public client |
| ২১ | OIDC: OAuth-এর ওপর Identity Layer | ID Token, claims ও validation |
| ২২ | Google Login এবং Token Validation | Google OIDC, token validation ও account linking |
| ২৩ | MFA এবং Step-up Authentication | Sensitive action ও assurance |
| ২৪ | Machine-to-Machine Authentication | Workload identity ও service credential |
| ২৫ | Context বদলালে Architecture কেন বদলায়? | Threat model ও architecture trade-off synthesis |
| ২৬ | Capstone: পুরো KenaKata Security Architecture | End-to-end architecture, abuse case ও residual risk |

এগুলো planning brief, পূর্ণাঙ্গ article নয়। OAuth-specific normative guidance অধ্যায় ১৭–২১-এ থাকবে। অধ্যায় ১৩–১৫-তে KenaKata-র first-party mobile refresh-token lifecycle শেখানো হবে; সেটিকে OAuth flow হিসেবে বর্ণনা করা হবে না।
