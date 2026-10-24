# Token কেন দরকার?

> অবস্থা: Scope নির্ধারিত। পূর্ণ অধ্যায় লেখা বাকি।

রাফি এখন ফোন থেকে KenaKata ব্যবহার করতে চায়। আগের server-rendered website-এ browser session cookie ছিল। নতুন mobile app-এ user-এর পরিচয় API request-এর সঙ্গে কীভাবে যাবে, সেটাই এই অধ্যায়ের প্রশ্ন।

## এই অধ্যায়ের শেষে পাঠক যা বুঝবে

- Session cookie ও access token দুটোই credential হিসেবে কাজ করতে পারে, কিন্তু client এগুলো কীভাবে বহন করে এবং server কীভাবে ব্যবহার করে, সেই model আলাদা।
- Native mobile app-এ access token কেন একটি উপযুক্ত API credential হতে পারে, আর cookie ব্যবহার করা একেবারেই অসম্ভব, এমন দাবি কেন ভুল।
- Access token দিয়ে protected resource request করার ধারণাগত flow।
- Token হাতে থাকা মানেই সব resource বা সব operation-এর অনুমতি থাকা নয়।
- Bearer token চুরি হলে কেন অন্য কেউ সেটি ব্যবহার করতে পারে।
- Token model নতুন সুবিধা আনে, কিন্তু credential storage, disclosure ও replay-এর ঝুঁকিও আনে।

## অধ্যায়ের সীমানা

এই অধ্যায় access token-এর প্রয়োজন ও basic mental model শেখাবে। JWT-এর গঠন অধ্যায় ৯-এ; token expiry ও replay timeline অধ্যায় ১১-এ; refresh token-এর প্রয়োজন, rotation ও revocation অধ্যায় ১২–১৪-এ; OAuth, PKCE ও OIDC অধ্যায় ১৬–২১-এ বিস্তারিত আসবে।

## লেখার অবস্থা

- [ ] Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
