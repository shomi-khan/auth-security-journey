---
chapter: 12
season: 1
title: "Refresh Token কেন?"
status: planned
---

# Refresh Token কেন?

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
অধ্যায় ১১-তে access token-এর মেয়াদ এবং replay window নিয়ে আলোচনা হয়েছে। এবার KenaKata-র mobile app-এ access token expire হলে প্রতিবার password দিয়ে login করানোর সমস্যা দেখা দেবে। Refresh token কীভাবে এই সমস্যার সমাধান করে, এবং কেন এটি access token-এর চেয়ে বেশি দীর্ঘমেয়াদি ও সংবেদনশীল credential, তা বোঝানো হবে।

## গল্পের শুরু
রাফি বাজারে থাকাকালীন mobile app-এ order দেখতে চায়। Access token expire করায় app তাকে বারবার login করতে বলছে। আরিফ প্রথমে access token-এর মেয়াদ বাড়াতে চায়। নীরা প্রশ্ন তোলে: মেয়াদ বাড়ালে token চুরি হওয়ার পর আক্রমণকারীর সময়ও কি বাড়বে না?

## আলোচ্য বিষয়
- Access token এবং refresh token-এর আলাদা কাজ ও trust boundary
- Refresh token কোথায় ব্যবহার হয়: token-issuing component-এ, resource API-তে নয়
- Expired access token থেকে নতুন access token পাওয়ার সাধারণ lifecycle
- Refresh token কেন দীর্ঘমেয়াদি এবং বেশি মূল্যবান credential
- Storage, transmission, TLS, access control, expiry এবং revocation-এর ভূমিকা
- Idle timeout ও absolute lifetime-এর পার্থক্য, এবং কেন কোনো একটি মেয়াদ সবার জন্য ঠিক নয়
- Refresh failure, expired/revoked credential এবং user-কে পুনরায় authenticate করানোর অভিজ্ঞতা
- KenaKata-র first-party mobile flow: এটিকে OAuth flow বলে উপস্থাপন করা হবে না

## অধ্যায় শেষে পাঠক পারবে
- Access token ও refresh token-এর কাজ আলাদা করে ব্যাখ্যা করতে
- Access token-এর মেয়াদ বাড়ানো আর refresh token ব্যবহার করার trade-off তুলনা করতে
- Refresh token চুরি হলে আক্রমণকারী কেন নতুন access token পেতে পারে তা ব্যাখ্যা করতে
- একটি refresh-token lifecycle-এর threat model ও প্রয়োজনীয় control চিহ্নিত করতে

## এই অধ্যায়ে থাকবে না
- OAuth Authorization Code Flow, PKCE বা OAuth-এর normative rules; এগুলো অধ্যায় ১৬–২০-এ
- Rotation ও reuse detection-এর পূর্ণ বিশ্লেষণ; অধ্যায় ১৩
- Logout, server-side revocation এবং session recovery-এর পূর্ণ নকশা; অধ্যায় ১৪
- নির্দিষ্ট framework বা library-র implementation tutorial

## ধারাবাহিকতা
আগে: [অধ্যায় ১১: Token Expiration এবং Replay Window](../11-token-expiration-replay/README.md)

পরে: [অধ্যায় ১৩: Refresh Token Rotation এবং Reuse Detection](../13-refresh-token-rotation/README.md)

Product timeline অনুযায়ী mobile app-এ refresh token আগে থেকেই আছে; SPA-র অধ্যায় ১০-এর সময় এই feature চালু। বিস্তারিত: [Product timeline](../../../product/timeline.md)。

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
