# Chapter 12 Editorial Brief

> Internal planning note. This file is not part of the reader-facing article.

## Chapter Brief: এই অধ্যায়ের ভূমিকা ও সীমা

**অধ্যায়ের কাজ:** Access token-এর মেয়াদ, token replay-এর সুযোগ, এবং token চুরি হলে আক্রমণকারীর হাতে কত সময় ও কী ক্ষমতা থাকে তা বোঝানো। Lifetime একা যথেষ্ট control নয়, এটি যেন পাঠক বুঝতে পারে।

**গল্পের কেন্দ্র:** KenaKata-র mobile access token expire হয়। আরিফ মেয়াদ কমিয়ে ঝুঁকি কমাতে চায়, কিন্তু user experience এবং token চুরি/পুনর্ব্যবহারের বাস্তব সীমা নিয়ে নীরার প্রশ্ন আসে।

**আলোচ্য বিষয়:** Expiration claim ও server-side validation; expiry বনাম revocation; clock skew ও validation-এর সীমা; replay window; token leakage-এর সম্ভাব্য জায়গা; short lifetime-এর usability/availability trade-off; exposure reduction, audience validation, least privilege এবং প্রয়োজন অনুযায়ী revocation বা sender-constraining-এর ভূমিকা।

**শেখার ফল:** পাঠক token expiration আর revocation আলাদা করতে পারবে, expiry কেন চুরি হওয়া token-কে সঙ্গে সঙ্গে বাতিল করে না তা ব্যাখ্যা করতে পারবে, এবং token lifetime-কে একা security solution না ধরে layered control-এর অংশ হিসেবে বিচার করতে পারবে।

**এই অধ্যায়ে থাকবে না:** Refresh-token lifecycle-এর পূর্ণ আলোচনা (অধ্যায় ১৩), rotation/reuse detection (অধ্যায় ১৪), logout/revocation recovery design (অধ্যায় ১৫), অথবা OAuth-specific normative flow (অধ্যায় ১৭–২১)। KenaKata-র mobile token flow-কে OAuth flow বলা হবে না।

**ধারাবাহিকতা:** আগে অধ্যায় ৯–১১-এ token, JWT ও SPA storage-এর mental model। পরে অধ্যায় ১৩-তে refresh token কেন দরকার তা আলোচনা হবে।

এই brief বিদ্যমান draft-এর scope স্পষ্ট করে; উপরের article ও review record মুছে বা প্রতিস্থাপন করে না।
