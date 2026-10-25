---
chapter: 23
season: 3
title: "MFA এবং Step-up Authentication"
status: planned
---

# MFA এবং Step-up Authentication

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ
একবার login হওয়া মানেই প্রতিটি সংবেদনশীল কাজের জন্য একই assurance যথেষ্ট নয়। Multi-Factor Authentication (MFA) এবং step-up authentication-এর উদ্দেশ্য, সীমা এবং user experience-এর trade-off ব্যাখ্যা করা হবে।

## গল্পের শুরু
এক কর্মীর session চুরি হয়েছে। আক্রমণকারী সাধারণ order দেখতে পারে, কিন্তু refund বা payout-এর মতো কাজও করতে পারবে কি? নীরা চায় সংবেদনশীল action-এর আগে অতিরিক্ত proof, তবে এমনভাবে নয় যে কর্মীরা নিয়ম পাশ কাটাতে বাধ্য হয়।

## আলোচ্য বিষয়
- Authentication factor: knowledge, possession, inherence
- MFA এবং একই factor-এর দুই ধাপের পার্থক্য
- Step-up authentication, recent authentication ও assurance context
- কোন action-এ step-up দরকার তা threat model ও product risk দিয়ে নির্ধারণ
- Phishing-resistant method-এর উচ্চস্তরের পরিচয় এবং OTP-এর সীমা
- Recovery code, lost device, account recovery ও help-desk risk
- Session/token-এ authentication time বা assurance context-এর ব্যবহার
- User friction, accessibility এবং recovery-এর trade-off
- MFA credential ও session compromise একই ঝুঁকি নয়

## অধ্যায় শেষে পাঠক পারবে
- MFA ও step-up-এর পার্থক্য বোঝাতে
- Sensitive action-এর জন্য risk-based policy প্রস্তাব করতে
- Recovery path-কে security design-এর অংশ হিসেবে ধরতে
- MFA চালু করলেই সব account takeover ঠেকে যায়, এমন দাবি এড়াতে

## এই অধ্যায়ে থাকবে না
- নির্দিষ্ট authenticator app-এর setup guide
- সম্পূর্ণ identity proofing system
- Machine-to-machine authentication; অধ্যায় ২৩

## ধারাবাহিকতা
আগে: [অধ্যায় ২১: Google Login এবং Token Validation](../../season-2-oauth-oidc/21-google-login/README.md)
পরে: [অধ্যায় ২৩: Machine-to-Machine Authentication](../24-machine-to-machine/README.md)

## লেখার অবস্থা
- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
