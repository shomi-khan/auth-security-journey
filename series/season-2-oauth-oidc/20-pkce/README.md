---
chapter: 20
season: 2
title: "PKCE"
status: planned
---

# PKCE

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
Public client-এ client secret গোপন রাখা যায় না। Authorization code অন্যের হাতে গেলেও সেটি যেন একা token পাওয়ার জন্য যথেষ্ট না হয়, PKCE সেই ঝুঁকিতে proof-of-possession-সদৃশ একটি transaction binding যোগ করে। কেন এটি কাজ করে, এবং কীভাবে ভুল implementation সুরক্ষা নষ্ট করে, তা বোঝানো হবে।

## গল্পের শুরু
একটি mobile app-এর callback থেকে authorization code কপি হয়ে যায়। আরিফ বলে client secret যোগ করলেই হবে। নীরা দেখায়, app bundle থেকে secret বের করা সম্ভব। তাই প্রতিটি authorization transaction-এর জন্য আলাদা verifier দরকার।

## আলোচ্য বিষয়
- Public client ও embedded secret-এর সীমাবদ্ধতা
- Code verifier এবং code challenge-এর সম্পর্ক
- `S256` transformation-এর ধারণা; plain mode-এর তুলনায় নিরাপত্তা
- Authorization request-এ challenge, token request-এ verifier
- Transaction binding: verifier কোথায় তৈরি ও রাখা হয়
- Intercepted code একা কেন যথেষ্ট হয় না
- `state` ও PKCE একে অন্যের বিকল্প নয়, যদিও কিছু flow-তে সম্পর্কিত protection দিতে পারে
- Verifier reuse, weak randomness, missing verifier validation-এর ঝুঁকি
- Native app ও browser-based public client-এর high-level প্রাসঙ্গিকতা

## অধ্যায় শেষে পাঠক পারবে
- PKCE sequence diagram ব্যাখ্যা করতে
- Code challenge ও verifier-এর ভূমিকা আলাদা করে বলতে
- কেন public client-এ static secret নির্ভরযোগ্য নয় তা বোঝাতে
- `state`, redirect URI ও PKCE-র আলাদা threat boundary চিহ্নিত করতে

## এই অধ্যায়ে থাকবে না
- OAuth server library-র implementation guide
- OIDC identity validation; অধ্যায় ২০
- Google login-এর পূর্ণ integration; অধ্যায় ২১

## ধারাবাহিকতা
আগে: [অধ্যায় ১৮: Redirect URI এবং Authorization Code Theft](../19-redirect-uri-code-theft/README.md)
পরে: [অধ্যায় ২০: OIDC](../21-oidc/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
