---
chapter: 12
season: 1
title: "Refresh Token কেন?"
status: planned
---

# Refresh Token কেন?

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ

অধ্যায় ১১-তে access token-এর মেয়াদ এবং replay window নিয়ে আলোচনা হয়েছে। এবার KenaKata-র mobile app-এ access token expire হলে প্রতিবার password দিয়ে login করানোর সমস্যা দেখা দেবে। Refresh token কীভাবে নতুন access token পাওয়ার সুযোগ দেয়, এবং কেন এটি সাধারণত দীর্ঘমেয়াদি ও বেশি সংবেদনশীল credential হতে পারে, তা বোঝানো হবে। Refresh token সব system-এ access token-এর চেয়ে বেশি দিন valid থাকবে, এমন কোনো universal rule নেই; lifetime system-এর policy ও risk-এর ওপর নির্ভর করে।

## গল্পের শুরু

রাফি বাজারে থাকাকালীন mobile app-এ order দেখতে চায়। Access token expire করায় app তাকে বারবার login করতে বলছে। আরিফ প্রথমে access token-এর মেয়াদ বাড়াতে চায়। নীরা প্রশ্ন তোলে: মেয়াদ বাড়ালে token চুরি হওয়ার পর আক্রমণকারীর সময়ও কি বাড়বে না?

## আলোচ্য বিষয়

- Access token এবং refresh token-এর আলাদা কাজ ও trust boundary
- Refresh token কোথায় ব্যবহার হয়: token-issuing component-এ, protected resource API-তে access token-এর বিকল্প হিসেবে নয়
- Expired access token থেকে নতুন access token পাওয়ার সাধারণ lifecycle
- Refresh token কেন সাধারণত বেশি দীর্ঘমেয়াদি এবং তাই বেশি মূল্যবান credential হতে পারে
- Refresh token চুরি হলে attacker নতুন access token পেতে পারে কেন, এবং এই ঝুঁকি কীভাবে সীমিত করা যায়
- Storage, transmission, TLS, access control, expiry এবং revocation-এর ভূমিকা; TLS transit-এ সাহায্য করে, storage নিরাপদ করে না
- Idle timeout ও absolute lifetime-এর পার্থক্য, এবং কেন কোনো একটি মেয়াদ সবার জন্য ঠিক নয়
- Refresh failure, expired/revoked credential এবং user-কে পুনরায় authenticate করানোর অভিজ্ঞতা
- KenaKata-র first-party mobile flow: এটিকে OAuth flow বলে উপস্থাপন করা হবে না

## অধ্যায় শেষে পাঠক পারবে

- Access token ও refresh token-এর কাজ আলাদা করে ব্যাখ্যা করতে
- Access token-এর মেয়াদ বাড়ানো আর refresh token ব্যবহার করার trade-off তুলনা করতে
- Refresh token চুরি হলে আক্রমণকারী কেন নতুন access token পেতে পারে তা ব্যাখ্যা করতে
- একটি refresh-token lifecycle-এর threat model ও প্রয়োজনীয় control চিহ্নিত করতে
- TLS, secure storage, expiry ও revocation কোন ঝুঁকি কমায় এবং কোন ঝুঁকি একা দূর করতে পারে না, তা বলতে

## Diagram ও security invariant

Mermaid sequence diagram-এ mobile app, token-issuing component এবং protected resource API-র আলাদা ভূমিকা দেখানো হবে। Expired access token-এর পর refresh সফল হওয়ার পথের পাশাপাশি refresh token expired, revoked বা invalid হলে কী ঘটে, সেই failure path-ও থাকবে। Diagram-এ OAuth Authorization Code Flow, PKCE, rotation, reuse detection বা logout recovery-কে Chapter 12-ன் flow-তে আগেভাগে আনা হবে না।

**মূল invariant:** Refresh token কেবল নতুন access token পাওয়ার জন্য নির্ধারিত trusted token-issuing component-এ পাঠানো হবে; protected resource-এ access চাওয়ার সময় access token ব্যবহার হবে। Refresh ব্যর্থ হলে client পুরোনো access token-কে বৈধ ধরে নেবে না, এবং policy অনুযায়ী user-কে পুনরায় authenticate করতে বলবে।

Diagram ধারণা শেখাবে, implementation নির্দেশনা দেবে না। কোনো language-specific code থাকবে না; pseudocode কেবল diagram দিয়ে মূল logic পরিষ্কার করা না গেলে বিবেচনা করা যাবে।

## এই অধ্যায়ে থাকবে না

- OAuth Authorization Code Flow, PKCE বা OAuth-এর normative rules; এগুলো অধ্যায় ১৬–২০-এ
- Rotation ও reuse detection-এর পূর্ণ বিশ্লেষণ; অধ্যায় ১৩
- Logout, server-side revocation এবং session recovery-এর পূর্ণ নকশা; অধ্যায় ১৪
- নির্দিষ্ট framework বা library-র implementation tutorial
- সব client বা system-এর জন্য প্রযোজ্য একক refresh-token lifetime বা একটিমাত্র storage strategy

## সূত্র যাচাইয়ের নির্দেশনা

- [RFC 6749 — The OAuth 2.0 Authorization Framework](https://www.rfc-editor.org/rfc/rfc6749): refresh token-এর সাধারণ ধারণা ও OAuth-এ token/resource server-এর ভূমিকা বুঝতে; KenaKata-র first-party flow-এর specification হিসেবে নয়।
- [RFC 9700 — Best Current Practice for OAuth 2.0 Security](https://www.rfc-editor.org/rfc/rfc9700): OAuth refresh-token security guidance-এর জন্য; এর normative requirement-গুলো KenaKata-র first-party flow-তে স্বয়ংক্রিয়ভাবে প্রযোজ্য বলে লেখা যাবে না।
- সংশ্লিষ্ট mobile storage ও credential-handling guidance-এর authoritative source draft লেখার সময় খুলে যাচাই করতে হবে।

## ধারাবাহিকতা

আগে: [অধ্যায় ১১: Token Expiration এবং Replay Window](../11-token-expiration-replay/README.md)

পরে: [অধ্যায় ১৩: Refresh Token Rotation এবং Reuse Detection](../13-refresh-token-rotation/README.md)

Product timeline অনুযায়ী mobile app-এ refresh token আগে থেকেই আছে; SPA-র অধ্যায় ১০-এর সময় এই feature চালু। বিস্তারিত: [Product timeline](../../../product/timeline.md)。

## লেখার অবস্থা

- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
