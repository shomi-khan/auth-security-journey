# অধ্যায় ৬-এর সঙ্গে repo-তে যা বদলাতে হবে (CONTRIBUTING.md অনুযায়ী)

অধ্যায়টা বসবে: `series/season-1-browser-session-token/06-csrf/README.md` (placeholder সরিয়ে)।
Status checkbox শুধু `Draft` টিক। Season/root README-তে `[ ] লেখা হয়নি` যেমন আছে তেমনই থাকবে, কারণ অধ্যায়টা এখনো published নয়।

## ১. notes/decision-log.md-এ নতুন এন্ট্রি

```
## 2026-10-14 — অধ্যায় ৬-এর দৃশ্য-সিদ্ধান্ত

- নীরার অনুরোধে অর্ডারের ইমেইলে একটা "বাতিল করুন" লিঙ্ক ছিল, আরিফ সেটা `GET` বানিয়েছিল। অধ্যায় ৬-এ সেটা ভুল প্রমাণিত হয়। এখন লিঙ্ক অর্ডারের পাতা খোলে (`GET`, শুধু দেখা), আর বাতিল হয় সেই পাতার `POST` বাটনে। এটা product.md-এর feature তালিকা বদলায় না।
- Session record-এ একটা CSRF token থাকে, session ID থেকে স্বাধীন random মান। login-এর আগের anonymous session-এও থাকে। অধ্যায় ৫-এর `Rotate` নতুন session বানালে নতুন token আসে।
- সবচেয়ে বাইরের middleware `crossSiteGuard`: `Sec-Fetch-Site`, না থাকলে `Origin` (server-এর কনফিগে লেখা target origin-এর সঙ্গে হুবহু মিল)। `same-site` প্রত্যাখ্যাত। দুটো header-ই না থাকলে প্রত্যাখ্যাত (fail-safe)। এটা Go 1.25-এর `http.CrossOriginProtection`-এর ডিফল্টের উল্টো, জেনেবুঝে।
- `/login` সুরক্ষিত guard আর anonymous session-এর token, দুটো দিয়ে।
- Logout `POST`, guard আর token-সহ (অধ্যায় ৪-এর ঋণ শোধ)।
- `blog.kenakata.example` অধ্যায় ৪-এর মতোই কাল্পনিক, KenaKata-র আসল নকশা নয়।
- দৃশ্যের অর্ডার-নম্বর ১০৫১ (অধ্যায় ৫-এর ১০৪২ নয়), যেন দুটো ঘটনার মধ্যে অনুমিত সংযোগ না তৈরি হয়।
```

## ২. notes/misconceptions.md-এ যোগ (এক লাইন করে)

- SameSite=Lax দিলেই CSRF শেষ
- শুধু POST ব্যবহার করলেই CSRF শেষ
- JSON API হলে CSRF হয় না
- CORS থাকলে CSRF থেকে নিরাপদ
- HttpOnly cookie CSRF ঠেকায়
- Referer দেখলেই যথেষ্ট
- নিশ্চিতকরণ পাতা বা multi-step থাকলেই CSRF ঠেকে
- CSRF token-এর মেয়াদ বা timestamp দরকার
- সাধারণ double-submit cookie যথেষ্ট
- session-এর মেয়াদ কম রাখলে CSRF হবে না
- login পাতায় CSRF লাগে না

## ৩. references/ ফাইলে যোগ (আগে থেকে আছে কি না দেখে নিয়ো; আমি references/owasp.md পড়তে পারিনি)

```
## Cross-Site Request Forgery Prevention Cheat Sheet

- কী বিষয়: synchronizer আর signed double-submit token, Fetch Metadata (`Sec-Fetch-Site`) আর Origin যাচাই, custom header, client-side CSRF, login CSRF, SameSite-এর সীমা।
- কোন অধ্যায়: ৬ (আর ১০, SPA-র সময়)।
- কেন পড়া useful: Fetch Metadata-র নীতি-সিদ্ধান্ত (fail-safe বনাম fail-open) আর rollout-এর পরামর্শ এখানেই।
- [CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
```

`references/browser-security.md`-তে:
- [MDN: Cross-site request forgery (CSRF)](https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/CSRF): ২০২৬-এর ২৯ সেপ্টেম্বর সর্বশেষ বদলানো দেখেছি। CSRF-এর তিন শর্ত, `Lax`-এর সীমা, method override।
- [Go: net/http CrossOriginProtection (go1.25)](https://pkg.go.dev/net/http@go1.25#CrossOriginProtection): browser-support আর vulnerability তালিকা বদলায়, "লেখার সময় দেখো" নোট রেখো।

## ৪. notes/glossary.md-তে যোগ (ছোট)

- **CSRF (Cross-Site Request Forgery):** victim-এর browser দিয়ে বিশ্বস্ত সাইটে জাল request করানো। বিস্তারিত অধ্যায় ৬।
- **Synchronizer token:** server-এর রাখা, session-নির্দিষ্ট, অনুমান-অযোগ্য মান, যা পাতায় বসে আর request-এ ফেরত আসে। অধ্যায় ৬।
- **Fetch Metadata (`Sec-Fetch-Site`):** browser-এর নিজে-বসানো header, যা request-এর উৎসের সম্পর্ক বলে। অধ্যায় ৬।

## ৫. Technical Review-এ যা মেলাতে হবে

- অধ্যায় ৫-এর `put` আর anonymous session শুরুর function: আমি তাদের ভেতরের কোড দেখিনি, নিজের পুনর্গঠন দিয়ে test করেছি। অধ্যায় ৬-এর §৪.৩-এর "put-এর ভেতরে একটা লাইন" আসল কোডের সঙ্গে মেলাও।
- দাবি: `Sec-` দিয়ে শুরু হওয়া header-ও JavaScript বদলাতে পারে না। OWASP-র সারাংশের ওপর নির্ভর, মূল Fetch Standard মেলানো হয়নি।
- দাবি (§৬): browser-এ HTTP Basic credential জমানো আর আপনা-আপনি পাঠানো। RFC 7617-এর একটা বাক্য মাত্র দেখেছি, আসল browser আচরণ পরীক্ষা করা হয়নি।
- Go 1.25-এর `CrossOriginProtection` আমি চালাইনি। pkg.go.dev-তে তালিকাভুক্ত GO-2025-3955 advisory পড়িনি।
- কোড শুধু Go 1.22.2-তে test হয়েছে। কোনো আসল browser-এ নয়।
- opening-এর দৃশ্য ("`POST` form-এর পরীক্ষা Lax আটকাল") গল্পের অংশ। আধুনিক browser-এ এটা সত্যি হওয়ার কথা, কিন্তু আমি browser চালিয়ে মেলাইনি।
