# OWASP

Cheat sheet বদলায়। লিংক ধরে রেখে অধ্যায় লেখার সময় পাতাটা আবার পড়ো। মনে থাকা পরামর্শকে অধ্যায়ে জমিয়ে রেখো না।

API-র তালিকা নতুন সংস্করণে গেলে যেন চোখে পড়ে, তাই প্রজেক্টের পাতাও রাখা আছে:

- [OWASP API Security Project](https://owasp.org/www-project-api-security/)

## Authentication Cheat Sheet

- কী বিষয়: Login, password, আর অ্যাকাউন্ট ধরে রাখার চর্চা।
- কোন অধ্যায়: ৩, আর পরে ২২ যখন MFA আসে।
- কেন পড়া useful: Password কীভাবে রাখা আর login কীভাবে ভাঙে, সেই সুপারিশ এখানে। সুপারিশ কপি করে অধ্যায়ে বসাবে না। পড়ে, KenaKata-র দৃশ্যের সঙ্গে মিলিয়ে লিখবে।
- [Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

## Session Management Cheat Sheet

- কী বিষয়: Session বানানো, cookie-তে রাখা, আর session বাতিল।
- কোন অধ্যায়: ৪ আর ৫।
- কেন পড়া useful: Cookie session নিয়ে কথা বলার আগে এটা খোলো।
- [Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

## CSRF Prevention Cheat Sheet

- কী বিষয়: Cross-site request forgery আটকানোর চর্চা।
- কোন অধ্যায়: ৬।
- কেন পড়া useful: CSRF কীভাবে আটকানোর কথা OWASP বলছে, সেটা অধ্যায়ের আগে পড়ো। কোন উপায় এখনকার ব্রাউজারে কী করে, সেটা browser-security রেফারেন্সের সঙ্গে মিলাও।
- [Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

## XSS Prevention Cheat Sheet

- কী বিষয়: Cross-site scripting আটকানোর চর্চা।
- কোন অধ্যায়: ৭।
- কেন পড়া useful: HttpOnly নিয়ে কথা বলার আগে XSS-এর পাতা পড়ো, যাতে flag-টাকে পুরো XSS-এর সমাধান বলে না বসে।
- [Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

## Authorization Cheat Sheet

- কী বিষয়: কে কোন কাজ করতে পারবে, সেই সিদ্ধান্ত কীভাবে শক্ত করা যায়।
- কোন অধ্যায়: ২ আর ১৫।
- কেন পড়া useful: IDOR আর BOLA-র অধ্যায়ের আগে এটা পড়ো। KenaKata-র Owner, Admin, Member, Viewer একটা সরল মডেল। Cheat sheet-এর সব সুপারিশ সরাসরি ওই মডেলে বসিয়ে দিয়ো না। মিল আর গরমিল অধ্যায়ে বলো।
- [Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)

পুরনো Access Control Cheat Sheet এখন deprecated। বর্তমান সোর্স হিসেবে সেটা ব্যবহার করবে না। OWASP নিজে Authorization Cheat Sheet-এ পাঠায়।

## API Security Top 10

এই ফাইল লেখার সময় OWASP-এর API-নির্দিষ্ট Top 10 ছিল 2023 সংস্করণ। পরের অধ্যায় সেটাকে “এখনকার” বলার আগে প্রজেক্টের পাতা আবার দেখো।

- কী বিষয়: API-তে যে ঝুঁকিগুলো বারবার দেখা যায়, তাদের তালিকা।
- কোন অধ্যায়: ১৫, ২৩, আর ২৪।
- কেন পড়া useful: BOLA-র নাম অধ্যায় ১৫-এর সঙ্গে মিলিয়ে রাখো। যে সংস্করণ পড়েছ, অধ্যায়ে সেই সংস্করণের কথা বলো।
- [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

## OAuth নিয়ে OWASP

- কী বিষয়: OAuth 2.0 নিয়ে OWASP-এর cheat sheet।
- কোন অধ্যায়: ১৬–২১।
- কেন পড়া useful: RFC 6749 আর RFC 9700-এর পাশে পড়ো। Cheat sheet আর RFC 9700 না মিললে RFC 9700 মানবে। মিল না থাকলে অধ্যায়ে সেটা লুকিয়ে ফেলবে না।
- [OAuth 2.0 Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html)
