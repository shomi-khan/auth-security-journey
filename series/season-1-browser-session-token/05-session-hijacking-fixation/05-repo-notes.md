# অধ্যায় ৫-এর সঙ্গে repo-তে যা বদলাতে হবে (CONTRIBUTING.md অনুযায়ী)

অধ্যায়টা বসবে: `series/season-1-browser-session-token/05-session-hijacking-fixation/README.md` (placeholder সরিয়ে)।
Status checkbox শুধু `Draft` টিক। Season/root README-তে `[ ] লেখা হয়নি` যেমন আছে তেমনই থাকবে, কারণ অধ্যায়টা এখনো published নয়।

## ১. notes/decision-log.md-এ নতুন এন্ট্রি (product-এ ছোট দৃশ্য-সংযোজন, তাই লিখে রাখা দরকার)

```
## 2026-10-12 — অধ্যায় ৫-এর দৃশ্য-সিদ্ধান্ত

- Login-এর আগে একটা anonymous session আছে, যাতে "login-এর পর ঠিক ওই পাতায় ফেরা" (return-to) কাজ করে। নীরার দাবি। এটা product.md-এর কোনো feature তালিকা বদলায় না, শুধু আচরণ।
- Session-এর মেয়াদ এখন তিন ঘড়ি: anonymous ৩০ মিনিট, idle ৩০ মিনিট, absolute ৮ ঘণ্টা (login থেকে)। অধ্যায় ৪-এর একক ৮ ঘণ্টার জায়গায়। এটা দৃশ্যের পছন্দ, সর্বজনীন সুপারিশ নয়।
- Login আর password বদলে session ID নতুন হয়। Password বদলে ওই user-এর বাকি সব session কাটে।
- Session-এর সঙ্গে login-এর সময়ের User-Agent আর network (IPv4 /24, IPv6 /64) জমা থাকে। বদল হলে session কাটে। এটা সংকেত, প্রমাণ নয়।
- অধ্যায় ৪-এর `Store.Lookup` এখন শুধু user ID নয়, পুরো record ফেরত দেয়।
- শুভ (user-4)-এর session চুরির ঘটনা: চুরির পথ কখনো জানা যায় না, ইচ্ছাকৃত।
```

## ২. notes/misconceptions.md-এ যোগ (এক লাইন করে, ব্যাখ্যা অধ্যায়েই)

- Idle timeout থাকলে hijack করা session বেশিক্ষণ চলে না
- Strict mode (server-এর বানানো ID-ই নেওয়া) থাকলে session fixation হয় না
- Login-এর পর শুধু session-এ user ID বসালেই হলো, ID বদলানো লাগে না
- IP আর User-Agent বাঁধলে session hijacking শেষ
- DBSC এলে cookie চুরি শেষ

## ৩. references/owasp.md-তে যোগ (এখন নেই)

```
## Cookie Theft Mitigation Cheat Sheet

- কী বিষয়: cookie চুরির পর শনাক্ত আর প্রতিক্রিয়া, DBSC-র সীমা।
- কোন অধ্যায়: ৫।
- কেন পড়া useful: login-এর পর চুরি-যাওয়া session নিয়ে কথা বলার আগে এটা খোলো। সংকেতের false positive আর negative সোর্সেই আছে।
- [Cookie Theft Mitigation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cookie_Theft_Mitigation_Cheat_Sheet.html)
```

আর `references/browser-security.md`-তে অধ্যায় ৫-এর জন্য WSTG-SESS-03 আর DBSC explainer (`https://github.com/w3c/webappsec-dbsc`) যোগ করা যায়।
DBSC-র browser-সমর্থন দ্রুত বদলায়, তাই ওখানে "লেখার সময় দেখো" নোট রেখো।

## ৪. notes/glossary.md-তে যোগ (ছোট)

- **Session Fixation:** login-এর আগে আক্রমণকারীর জানা session ID-তেই victim login করে। বিস্তারিত অধ্যায় ৫।
- **Session Hijacking:** login-এর পরে চালু session ID চুরি করে ব্যবহার করা। অধ্যায় ৫।
- **Idle / Absolute timeout:** শেষ activity-র পর বনাম login-এর পর মেয়াদ। অধ্যায় ৫।

## ৫. Technical Review-এ যা মেলাতে হবে

- Constella-র সংখ্যা (§৩): ভেন্ডর-ব্লগ থেকে নেওয়া। প্রাথমিক সূত্রে মেলাও বা সংখ্যা বাদ দাও।
- DBSC-র অবস্থা (§৮): Help Net Security আর Workspace Updates থেকে। প্রকাশের আগে Chrome-এর platform status আর W3C পাতা দেখো।
- Context-signal-এর ছক (§৭) KenaKata-র মতো ছোট দোকানের জন্য ঠিক, বড় mobile-নির্ভর অ্যাপের জন্য নয়। অধ্যায়ে সেটা বলা আছে, কিন্তু তোমার চোখে ঠিক লাগল কি না দেখো।
