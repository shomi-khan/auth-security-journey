---
chapter: 4
season: 1
title: "Session এবং Cookie"
status: draft
---

# Session এবং Cookie

সোমবার সকাল। ফাঁসের ঘটনার পর KenaKata-র প্রথম পুরো কর্মদিবস।

রাফি দোকান খুলে নতুন passphrase টাইপ করে login করল। Argon2id তাকে চিনল। তারপর সে অর্ডারের তালিকা খুলল, একটা অর্ডারে ঢুকল, ফিরে এল, আরেকটায় ঢুকল। নীরা ফোনে তার সঙ্গে কথা বলছিল।

"পাতাগুলো আগের চেয়ে একটু ভারী লাগছে," রাফি বলল।

আরিফ server-এর CPU-র গ্রাফ খুলল। কারণ খুঁজতে হলো না। একটা পাতা খুলতে browser পাঠায় প্রায় দশটা request, আর অধ্যায় ১-এর সেই নকশায় প্রতিটার সঙ্গে রাফির passphrase যাচ্ছে। প্রতিটায় নতুন করে Argon2id, প্রায় ৪০–৫০ ms CPU আর ১৯ MiB memory, ইচ্ছা করেই দামি। দশ request মানে শুধু "রাফি আসলেই রাফি" বলার জন্য প্রায় আধ সেকেন্ডের CPU।

নীরা আরেকটা কথা তুলল। "আর কর্মীরা সারাদিন একই কাজ করে। প্রতিটা পাতায় passphrase চাইলে ওরা কাগজে লিখে মনিটরে সাঁটবে।"

নীরা ঠিকই বলছিল। অধ্যায় ২-এ আমরা দেখেছিলাম, নিরাপত্তার নিয়ম কাজে এতটা বাধা হলে মানুষ পাশ কাটায়।

আরিফ `loginHandler`-এর শেষ লাইনটা আবার পড়ল: `_ = id`। ফাংশনটা একজন মানুষকে চিনল, তারপর চেনার ফলটা ছুড়ে ফেলল।

---

## ১. কী চাই, আর কী চাই না

অধ্যায় ১-এ আমরা দুটো কাজ আলাদা করেছিলাম: identity **প্রমাণ করা** আর প্রমাণিত identity **ধরে রাখা**। প্রথমটা আমরা অধ্যায় ৩-এ ঠিকমতো বানিয়েছি, ভারী আর ধীর, কারণ সেটা হাতে-গোনা বার চলে। এবার দ্বিতীয় কাজটা।

আরিফ তার চাহিদাগুলো লিখল:

1. **হালকা।** প্রতিটা request-এ বইবার মতো। Argon2id-র মতো নয়।
2. **password নয়।** password প্রতি request-এ ঘুরলে অধ্যায় ১-এর ভুলে ফেরা হয়।
3. **চুরি হলে আগের জায়গায় ফেরা নয়।** অধ্যায় ৩-এর শেষ প্রশ্নটা মনে আছে? যে চিহ্ন প্রতিটা request-এ যায়, সেটা চুরি হলে আমরা কি আবার সেই জায়গায়, যেখান থেকে বেরিয়ে এলাম? অন্তত এটা এমন হওয়া চাই, যা server বাতিল করতে পারে।

browser-এ ঠিক এই কাজের জন্য একটা যন্ত্র আছে। নাম **cookie**।

### Cookie কী

**Cookie** হলো browser-এর একটা কৌশল। server একটা ছোট `নাম=মান` জোড়া browser-কে দেয়, browser সেটা জমিয়ে রাখে, আর পরের request-গুলোতে (নির্দিষ্ট নিয়ম মেনে) ফেরত পাঠায়।

```http
HTTP/1.1 200 OK
Set-Cookie: lang=bn
```

```http
GET /products HTTP/1.1
Host: kenakata.example
Cookie: lang=bn
```

`Set-Cookie` হলো server-এর response header: "এটা জমিয়ে রাখো।" `Cookie` হলো browser-এর request header: "তুমি যেটা দিয়েছিলে, এই নাও।" কখন browser ফেরত পাঠাবে, সেই নিয়মগুলো §৭-এ। আপাতত শুধু এটুকু: **server অনুরোধ করে, সিদ্ধান্ত নেয় browser।**

> **একটু পেছনে তাকাই।** ১৯৯৪ সালের জুনে Netscape-এর প্রোগ্রামার Lou Montulli একটা সমস্যায় পড়েছিলেন। Netscape তখন MCI-র জন্য একটা e-commerce application বানাচ্ছিল, আর MCI চায়নি তাদের server অসমাপ্ত লেনদেনের অবস্থা নিজে মনে রাখুক। সমাধান: অবস্থাটা ক্রেতার নিজের browser-এ রাখো। virtual shopping cart-এর জন্য cookie-র জন্ম এভাবেই। (ইতিহাসের এই অংশ Wikipedia-র "HTTP cookie" লেখা থেকে। এটা ইতিহাস, security-র দাবি নয়।)
>
> KenaKata-ও একটা দোকান, তাই মিলটা মজার। আর একটা তিক্ত পরিহাসও আছে। cookie জন্মেছিল server-কে মনে রাখার দায় থেকে মুক্তি দিতে। আমরা আজ ঠিক উল্টো পথে হাঁটব: server মনে রাখবে, cookie শুধু বইবে। কেন, সেটা §২ থেকে §৪-এ।

আর এই অধ্যায়ের সবচেয়ে গুরুত্বপূর্ণ বাক্য সম্ভবত এটাই:

> **Cookie নিজে authentication system নয়।** এটা একটা বাহক। কী বইছে, সেটাই আসল প্রশ্ন।

---

## ২. প্রথম ভাবনা: cookie-তে "আমি কে" লিখে দিই

আরিফের প্রথম সমাধান সবচেয়ে সোজাটা। login সফল হলে user-এর ID cookie-তে বসিয়ে দাও। পরের request-এ cookie দেখে বলো, এ কে।

```go
// আরিফের প্রথম সংস্করণ। কাজ করে, কিন্তু ভুল।
func loginHandler(w http.ResponseWriter, r *http.Request) {
	id, ok := verifyPassword(r.FormValue("email"), r.FormValue("password"))
	if !ok {
		http.Error(w, "email বা password ঠিক নয়", http.StatusUnauthorized)
		return
	}
	http.SetCookie(w, &http.Cookie{Name: "user", Value: id.UserID, Path: "/"})
	http.Redirect(w, r, "/manage/orders", http.StatusSeeOther)
}

func authenticate(r *http.Request) (Identity, bool) {
	c, err := r.Cookie("user")
	if err != nil {
		return Identity{}, false
	}
	return Identity{UserID: c.Value}, true // cookie যা বলছে, তা-ই বিশ্বাস
}
```

হালকা, সোজা, আর password আর ঘুরছে না। আরিফ test করল: রাফি login করলে পরের পাতাগুলো খোলে। কাজ শেষ?

এবার আরিফ একটা নতুন অভ্যাস কাজে লাগাল, অধ্যায় ৩ থেকে শেখা। deploy করার আগে সে আক্রমণকারীর চেয়ারে বসল। সে নিজেকে জিজ্ঞেস করল: **এই cookie-র প্রতিটা byte কার হাতে?**

উত্তরটা অস্বস্তিকর। cookie ফিরে আসে browser থেকে, আর browser বসে user-এর হাতে। RFC 6265-ও স্পষ্ট বলে: একটা ক্ষতিকর client `Cookie` header পাঠানোর আগে সেটা বদলে দিতে পারে।

আরিফ browser-এর developer tools খুলল। Application ট্যাবে cookie-র তালিকায় `user | user-7`। সে মানটা বদলে লিখল `user-1`, পাতা reload করল।

পাতার ওপরে লেখা: *স্বাগতম, রাফি।*

সময় লাগল পাঁচ সেকেন্ড। `curl` দিয়েও একই কাজ এক লাইনে হয়:

```bash
curl -i -H 'Cookie: user=user-1' https://kenakata.example/manage/orders
```

ভেতরে ঢোকা গেল। আর অধ্যায় ২-এর `user-1` আর `user-7` নাম দুটো তো URL-এই লেখা থাকত (`/manage/users/user-7/profile`)। আক্রমণকারীকে আন্দাজও করতে হলো না।

এখানে কয়েকটা জিনিস একসঙ্গে ভাঙল।

**authorization ঠিকই কাজ করেছে।** `can(user-1, ViewProfile)` সত্যিই `true` দেয়, কারণ ওই পরিচয়ের সেই অনুমতি আছে। ভুল ছিল পরিচয়ে। অধ্যায় ২-এর শেষ নীতিটা এখানে ফিরে এল: request-এ আসা ID একটা **দাবি**, প্রমাণ নয়। ছদ্মবেশ শুধু বদলেছে। আগে ID ছিল path-এ, এখন cookie-তে।

**log মিথ্যে বলবে।** অধ্যায় ৩-এ দেখেছিলাম, পরিচয় জাল হলে log-এ `by=user-1` লেখা হবে আর সেটা সত্যি দেখাবে। accountability আবার শূন্যে।

**শেখার জিনিস:** শুধু একটা ID দিয়ে "আমি কে" বোঝানো যায় না, কারণ ID জানা মানেই তা ব্যবহার করা যাওয়া। আমাদের দরকার এমন একটা জিনিস, যা **জানা যায় না**।

---

## ৩. দ্বিতীয় ভাবনা: cookie-তে সই করে দিই

আরিফ একটা ব্লগে পড়েছিল, cookie-তে সই (signature) জুড়ে দিতে হয়। ধারণাটা সহজ। server-এর কাছে একটা গোপন চাবি থাকে। cookie-র মান হয় `user-7` আর তার একটা সই (যেমন HMAC), এরকম:

```
user=user-7.9f2c41…
```

server cookie ফেরত পেলে সই আবার হিসাব করে মেলায়। চাবি না জানলে কেউ `user-1`-এর সই বানাতে পারবে না। আর server-এর কিছু মনে রাখতে হচ্ছে না, ঠিক যেমন MCI চেয়েছিল।

জালিয়াতি তো গেল। কিন্তু আরিফ আবার আক্রমণকারীর চেয়ারে বসল, আর তিনটা প্রশ্ন বেরোল।

**এক: কেউ আসল cookie-টাই নিয়ে নিলে?** সই প্রমাণ করে, cookie-টা আমাদের বানানো। প্রমাণ করে না, এটা সেই মানুষটার হাতেই আছে। RFC 6265 নিজেই বলে, cookie-র ভেতরের অংশ encrypt বা sign করলেও আক্রমণকারী এক user agent থেকে আরেকটায় cookie বসিয়ে দিতে বা পরে আবার চালাতে (replay) পারে।

**দুই: বাতিল করব কীভাবে?** ধরো রাফি বুধবার একজন কর্মীকে ছাড়িয়ে দিল। অধ্যায় ২-এর শেষে প্রশ্নটা খোলা রেখেছিলাম: তার চলতে থাকা login-এর কী হবে? সই-করা cookie নিজেই নিজের প্রমাণ। তার মেয়াদ cookie-র ভেতরে লেখা, আর সেটাও সই করা। server-এর কাছে বাতিলের কোনো জায়গা নেই। চাবি বদলালে সবাই বেরিয়ে যায়। একজনকে আলাদা করে বের করতে হলে server-কে আবার মনে রাখতে হয় কারা বাতিল। মানে state ফিরে এল।

**তিন: চাবি ফাঁস হলে?** তখন আক্রমণকারী যে কোনো user-এর cookie বানাতে পারে, চুপচাপ। একটা চাবি, সবার চাবি।

এটা ভুল নকশা নয়। এটা **অন্য একটা trade-off**: server-এর স্মৃতি সরানোর বিনিময়ে বাতিল করার ক্ষমতা আর ক্ষতির সীমা কমে। এই ধারণা পরে ফিরবে token আর JWT-র আলোচনায় (অধ্যায় ৮–৯)। এখন KenaKata-র যে দরকার (কর্মী ছাড়ানো, চুরি-যাওয়া চিহ্ন বাতিল), সেটা এই নকশা দেয় না।

---

## ৪. তৃতীয় ভাবনা: server-এর খাতা, cookie-তে শুধু একটা নম্বর

ভাবনাটা উল্টে দেওয়া যাক। cookie-তে "আমি কে" লেখা থাকবে না। থাকবে শুধু একটা লম্বা, এলোমেলো নম্বর, যার নিজের কোনো মানে নেই। মানেটা থাকবে **server-এর খাতায়**।

অধ্যায় ১-এর wristband মনে আছে? এবার wristband-এ কোনো নাম লেখা নেই, শুধু একটা নম্বর। গেটের খাতায় নম্বরের পাশে লেখা: *user-1, ঢুকেছে ৯:০২-এ, মেয়াদ সন্ধ্যা ৫টা।* wristband হারালে সেই নম্বর বাতিল করে নতুন দেওয়া যায়। কর্মী ছাড়ালে খাতা থেকে লাইনটা কেটে দিলেই হলো।

এই খাতার নাম **session**। নম্বরটা **session ID**। RFC 6265 এই পদ্ধতিকে প্রচলিত বলে: cookie-তে তথ্য না রেখে একটা nonce রাখো, server সেই nonce দিয়ে নিজের তথ্য খোঁজে। আর তার একটা সুবিধাও স্পষ্ট করে: আক্রমণকারী cookie-র মান জেনে ফেললেও সেটা শুধু ওই server-এর সঙ্গে কথা বলতেই কাজে লাগে, মানের ভেতরে কোনো সংবেদনশীল তথ্য নেই।

> **একটা দার্শনিক টীকা।** server কখনো রাফিকে "চেনে" না। সে চেনে একটা নম্বর। পরিচয়ের ধারাবাহিকতা মানে হলো একই গোপন নম্বরের বারবার ফিরে আসা। অধ্যায় ১-এর Memento-র নায়কের মতো: নোট যার হাতে, সে-ই আমি। তাই নোটটা কতটা গোপন, কতটা বাতিলযোগ্য, সেটাই পুরো নকশা।

তিনটা আলাদা জিনিস, তিনটা আলাদা নাম। গুলিয়ে ফেললে পরের অধ্যায়গুলো গুলিয়ে যাবে:

| নাম | কী | কোথায় থাকে |
|---|---|---|
| **Session** | login-এর রেকর্ড: কে, কখন, কতক্ষণ | server-এর খাতায় (store) |
| **Session ID** | ওই রেকর্ডের চাবি, এলোমেলো নম্বর | browser-এর হাতে |
| **Cookie** | session ID বইবার browser-এর কৌশল | browser-এর cookie jar |

cookie-তে session ID ছাড়া আর কিছুও থাকতে পারে (যেমন ভাষার পছন্দ), আবার session ID অন্য উপায়েও যেতে পারে। দুটো এক জিনিস নয়।

প্রবাহটা ছবিতে দেখি। প্রশ্নটা হলো: কোন কাজ একবার হয়, আর কোনটা প্রতি request-এ?

```mermaid
sequenceDiagram
    actor Staff as কর্মী
    participant B as Browser
    participant S as KenaKata server
    participant St as Session store

    Note over Staff,St: একবার: identity প্রমাণ
    Staff->>B: email আর passphrase লেখে
    B->>S: POST /login (HTTPS)
    S->>S: Argon2id দিয়ে password যাচাই
    S->>S: crypto/rand দিয়ে নতুন session ID বানায়
    S->>St: hash(session ID) → user-1, মেয়াদ
    S-->>B: 303 আর Set-Cookie: __Host-id=session ID
    B->>B: cookie jar-এ রাখে

    Note over Staff,St: প্রতিটা request: হালকা যাচাই
    B->>S: GET /manage/orders আর Cookie: __Host-id=session ID
    S->>St: hash(session ID) খোঁজো
    St-->>S: user-1, মেয়াদ আছে
    S-->>B: অর্ডারের তালিকা
```

ছবির তিনটা জিনিস লক্ষ করার মতো।

**প্রথমত,** password একবারই ঘুরেছে, `POST /login`-এ। তারপর থেকে পথে ঘুরছে শুধু session ID। ভারী কাজ (Argon2id) একবার, হালকা কাজ (একটা খোঁজ) প্রতিবার।

**দ্বিতীয়ত,** session ID বানাচ্ছে **server**, user নয়। client কোনো নম্বর বেছে নিতে পারে না। এই ছোট সিদ্ধান্তটা অধ্যায় ৫-এ বড় হয়ে ফিরবে।

**তৃতীয়ত,** store-এ session ID নয়, তার **hash** যাচ্ছে। কেন, সেটা §৬-এ।

### তিন নকশা, পাশাপাশি

অধ্যায় ১-এ আমরা দুই ভুল চাল পাশাপাশি দেখেছিলাম। এখানে তিনটা:

| প্রশ্ন | `user=user-1` | সই-করা cookie | random session ID |
|---|---|---|---|
| কে মনে রাখে? | কেউ না | client | server |
| জাল করা যায়? | সঙ্গে সঙ্গে | চাবি ছাড়া না | অনুমান করতে হবে (§৫) |
| cookie চুরি হলে replay? | হ্যাঁ | হ্যাঁ | হ্যাঁ |
| একজনকে আলাদা বাতিল? | না | না (চাবি বদলালে সবাই) | হ্যাঁ, একটা row মুছে |
| ফাঁসের ক্ষতি | যে কেউ যে কেউ | চাবি গেলে সবাই | store-এ hash থাকলে read-only ফাঁসে শূন্য (§৬) |
| server-এর খরচ | নেই | নেই | প্রতি request-এ একটা খোঁজ |

তৃতীয় সারিটা সবার এক, আর এটা মনে রাখা জরুরি। session ID-ও চুরি করে ব্যবহার করা যায়। আমরা চুরি আটকাইনি। বদলেছে চুরির **পরের** অবস্থা: কতক্ষণ, কতটুকু, আর বাতিল করা যায় কি না।


## 🧠 Decision Game: বুধবার কর্মীকে ছাড়ানো হলো

রাফির একজন কর্মী, `user-7`, আর KenaKata-তে কাজ করবে না। তার browser-এ এখনো login খোলা। কোন নকশায় অন্য সবার login অক্ষত রেখে শুধু তার চলতি session বাতিল করা যায়?

- **A.** Cookie-র মেয়াদ browser-এ শেষ হওয়া পর্যন্ত অপেক্ষা করা
- **B.** Server-এর session store থেকে `user-7`-এর session record মুছে দেওয়া
- **C.** সব user-এর cookie-signing key বদলে দেওয়া

উত্তর দেওয়ার আগে ভাবো: browser-এর কাছে থাকা কপিটা মুছতে পারা আর server-এর কাছে credential-টাকে অকার্যকর করতে পারা কি একই ব্যাপার?

<details>
<summary>উত্তর ও ব্যাখ্যা দেখো</summary>

**সঠিক উত্তর: B।** Server-side session-এ server-এর খাতায় session ID-র সঙ্গে user-এর সম্পর্ক থাকে। `user-7`-এর record মুছে দিলে পরের request-এ ওই ID আর বৈধ session খুঁজে পাবে না, কিন্তু অন্য user-দের session চলবে।

A শুধু browser-এর কপি সরাতে পারে; চুরি-যাওয়া কপি তখনও কাজ করতে পারে। C সই-করা cookie-র ক্ষেত্রে সবাইকে প্রভাবিত করতে পারে, কিন্তু আলাদা user-কে বাতিল করার সুবিধা দেয় না।

**Edge case:** একই user একাধিক device-এ login করে থাকলে, শুধু একটি session মুছবে নাকি তার সব session বাতিল করবে, সেটাও policy-তে স্পষ্ট থাকতে হবে।

</details>

---

## ৫. Session ID কেমন হবে

session ID-র গুণ নিয়ে OWASP-র Session Management Cheat Sheet পরিষ্কার। আমাদের কাজের কথাগুলো:

- **অনুমান-অযোগ্য।** cryptographically secure random generator (CSPRNG) থেকে আসতে হবে, আর কমপক্ষে ৬৪ bit entropy থাকতে হবে। নিজে বানালে OWASP বলে কমপক্ষে ১২৮ bit-এর CSPRNG মান। আমরা নেব ৩২ byte, মানে ২৫৬ bit। `math/rand` চলবে না (অধ্যায় ৩-এর salt-এর মতোই), কারণ দুর্বল generator-এর মান statistical বিশ্লেষণে অনুমান করা যেতে পারে।
- **অর্থহীন।** মানের ভেতরে user ID, role, ইমেল, কিছুই নয়। অর্থ থাকবে শুধু server-এর খাতায়।
- **server-এর বানানো।** server যে ID কখনো বানায়নি, সেটা পেলে প্রত্যাখ্যান। OWASP একে strict mode বলে।
- **generic নাম।** `PHPSESSID` বা `JSESSIONID`-এর মতো framework-এর ডিফল্ট নাম বলে দেয় কোন প্রযুক্তি চলছে। আমরা নাম রাখব `__Host-id`। `__Host-`-এর মানে §৭-এ।

### কেন URL-এ নয়, cookie-তে?

অধ্যায় ১-এর গোপন URL মনে আছে? session ID URL-এ রাখলে ঠিক সেই পুরোনো ফাঁসগুলো: browser history, server log, `Referer` header, শেয়ার করা লিঙ্ক। OWASP-ও এই কারণ দেখায় আর বলে, session ID শুধু cookie দিয়ে গ্রহণ করাই ভালো, URL parameter থেকে নয়। কারণ URL-এ নেওয়া মানে আক্রমণকারী আগে থেকে ID বসিয়ে দিতে পারে (অধ্যায় ৫-এ আসবে)।

### কেন cookie-তে, `Authorization` header-এ নয়?

অধ্যায় ১-এ HTTP Basic প্রতি request-এ `Authorization` header-এ password পাঠাত। session ID-ও header-এ পাঠানো যায়। তফাতটা একটাই, কিন্তু ভারী:

| | Cookie | `Authorization` header |
|---|---|---|
| কে পাঠায়? | **browser নিজে**, নিয়ম মেনে, স্বয়ংক্রিয়ভাবে | **আমাদের কোড**, প্রতিটা request-এ জুড়ে দিতে হয় |
| JavaScript পড়তে পারে? | `HttpOnly` থাকলে না | যেখানে রাখা হবে সেখান থেকে পারে |
| অন্য সাইট থেকে request এলে? | browser তবু পাঠাতে পারে (অধ্যায় ৬) | পাঠায় না, কারণ সেই সাইটের কাছে মানটা নেই |

browser নিজে পাঠায়, এই সুবিধাটাই আবার সমস্যা। RFC 6265 এই বৈশিষ্ট্যের নাম দেয় **ambient authority**। request কে শুরু করেছে, সেটা না দেখে browser authority জুড়ে দেয়। অধ্যায় ৬-এর পুরো গল্প এটা। আর অধ্যায় ৮ থেকে ১০ দেখাবে, header-এ রাখলে কোন সমস্যা কমে আর কোনটা নতুন করে আসে।

---

## ৬. কোড: খাতা, cookie, আর অধ্যায় ১-এর ফাঁকা `authenticate`

এই কোড আমি Go 1.22.2-তে compile করে, test চালিয়ে দেখেছি (§তথ্যসূত্রে বিস্তারিত)। প্রথমে খাতা। `Store` একটা `map`, production-এ এটা database বা Redis-এর মতো কিছু হবে। ধারণাটা এক।

```go
package main

import (
	"crypto/rand"
	"crypto/sha256"
	"encoding/base64"
	"encoding/hex"
	"sync"
	"time"
)

const (
	tokenBytes  = 32            // ২৫৬ bit
	sessionLife = 8 * time.Hour // এই অধ্যায়ের সরল ছক: একটাই মেয়াদ। idle আর absolute আলাদা হবে অধ্যায় ৫-এ
)

type record struct {
	UserID    string
	CreatedAt time.Time
	ExpiresAt time.Time
}

// Store: key হলো session ID-র hash, session ID নিজে নয়।
type Store struct {
	mu   sync.Mutex
	rows map[string]record
	now  func() time.Time
}

func NewStore() *Store {
	return &Store{rows: map[string]record{}, now: time.Now}
}

func digest(token string) string {
	sum := sha256.Sum256([]byte(token))
	return hex.EncodeToString(sum[:])
}

// Create নতুন random session ID বানায়। store-এ যায় শুধু তার hash।
// ID ফেরত যায় একবারই, cookie-তে বসানোর জন্য।
func (s *Store) Create(userID string) (string, error) {
	b := make([]byte, tokenBytes)
	if _, err := rand.Read(b); err != nil { // crypto/rand, math/rand নয়
		return "", err
	}
	token := base64.RawURLEncoding.EncodeToString(b)

	now := s.now()
	s.mu.Lock()
	s.rows[digest(token)] = record{UserID: userID, CreatedAt: now, ExpiresAt: now.Add(sessionLife)}
	s.mu.Unlock()
	return token, nil
}

// Lookup: cookie থেকে আসা ID কোন user-এর? না পেলে বা মেয়াদ গেলে ok=false।
func (s *Store) Lookup(token string) (string, bool) {
	key := digest(token)

	s.mu.Lock()
	defer s.mu.Unlock()
	r, found := s.rows[key]
	if !found {
		return "", false
	}
	if !s.now().Before(r.ExpiresAt) {
		delete(s.rows, key)
		return "", false
	}
	return r.UserID, true
}
```

কোডে নাম `token`, কারণ এটা random, অনুমান-অযোগ্য মান। মনে রেখো, এটাই session ID। তারপর cookie বসানো:

```go
const cookieName = "__Host-id"

func setSessionCookie(w http.ResponseWriter, token string) {
	http.SetCookie(w, &http.Cookie{
		Name:     cookieName,
		Value:    token,
		Path:     "/",                  // __Host- prefix-এর শর্ত
		Secure:   true,                 // শুধু HTTPS-এ যাবে
		HttpOnly: true,                 // document.cookie-তে দেখা যাবে না
		SameSite: http.SameSiteLaxMode, // ডিফল্টের ওপর ভরসা নয়, স্পষ্ট করে লেখা
		// Domain নেই: __Host- prefix-এ Domain থাকা চলে না। cookie শুধু এই host-এ
		// MaxAge/Expires নেই: browser বন্ধ হলে cookie যাওয়ার কথা। আসল মেয়াদ server-এর খাতায়
	})
}
```

এই একটা ফাংশন যে সিদ্ধান্তগুলো বয়ে আনছে, সেগুলো পরের অংশে একে একে খুলব। আগে অধ্যায় ১-এর ফাঁকা `authenticate` আর পাহারাদার:

```go
func authenticate(r *http.Request) (Identity, bool) {
	c, err := r.Cookie(cookieName)
	if err != nil {
		return Identity{}, false
	}
	userID, ok := sessions.Lookup(c.Value)
	if !ok {
		return Identity{}, false
	}
	return Identity{UserID: userID}, true
}

func requireAuthentication(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		id, ok := authenticate(r)
		if !ok {
			if r.Method == http.MethodGet { // browser-এর পাতা: login পাতায় পাঠাই
				http.Redirect(w, r, "/login", http.StatusFound)
				return
			}
			http.Error(w, "authentication required", http.StatusUnauthorized)
			return
		}
		ctx := context.WithValue(r.Context(), identityKey{}, id)
		next.ServeHTTP(w, r.WithContext(ctx))
	})
}
```

আর `loginHandler`-এর সেই `_ = id` লাইন এখন এরকম:

```go
func loginHandler(w http.ResponseWriter, r *http.Request) {
	r.Body = http.MaxBytesReader(w, r.Body, 4<<10)
	id, ok := verifyPassword(r.FormValue("email"), r.FormValue("password"))
	if !ok {
		http.Error(w, "email বা password ঠিক নয়", http.StatusUnauthorized)
		return
	}
	token, err := sessions.Create(id.UserID)
	if err != nil {
		http.Error(w, "server error", http.StatusInternalServerError)
		return
	}
	setSessionCookie(w, token)
	w.Header().Set("Cache-Control", "no-store") // session ID সহ response কোনো cache-এ জমবে না
	http.Redirect(w, r, "/manage/orders", http.StatusSeeOther)
}
```

আমার test-এ একটা আসল login-এর response header এমন এসেছিল (ID-টা এক-ব্যবহারের নমুনা):

```
Set-Cookie: __Host-id=jd3COo2PDshkWEsI2mswgz39yB6IXw9k1_zW4jdIN2o; Path=/; HttpOnly; Secure; SameSite=Lax
```

ID ৪৩ অক্ষরের। store-এ যে key বসেছিল, সেটা এর SHA-256, ৬৪ hex অক্ষরের `c9ae14…edebe`। ID নিজে store-এ নেই।

### কোডের পেছনের সিদ্ধান্তগুলো

**store-এ hash কেন?** অধ্যায় ৩-এ ডাটাবেসের ফাঁস আমাদের ঘুম কেড়েছিল। ধরো আবার কোনোদিন একটা backup বেরিয়ে গেল। store-এ যদি আসল session ID থাকে, আক্রমণকারী সেগুলো cookie হিসেবে বসিয়ে সব চালু session-এ ঢুকে পড়বে, password ছাড়াই। OWASP-র কথা: শক্ত random ID অনুমান আটকায়, কিন্তু ফাঁস-হওয়া ID আটকায় না। তাই read-only ফাঁস যদি threat model-এ থাকে (আমাদের আছে), store-এ one-way hash রাখো।

আমার test-এও এটা দেখেছি: store-এর key, মানে hash-টাকেই cookie হিসেবে পাঠালে server ফিরিয়ে দেয়, কারণ সে hash-এর hash খোঁজে।

**Argon2id নয় কেন, SHA-256 কেন?** অধ্যায় ৩-এর তালিকায় লিখেছিলাম, যন্ত্রের বানানো লম্বা random মানের জন্য ধীর hash দরকার হয় না, আর এই অধ্যায়ে এসে বলব কেন। password ধীর hash চায়, কারণ আক্রমণকারী **অনুমান** করে আর ছোট একটা সম্ভাবনার জায়গায়: `rafi1985`, `shuvo@123`। ধীর hash প্রতিটা অনুমান দামি করে। আমাদের session ID-র সম্ভাবনার জায়গা 2²⁵⁶, কেউ অনুমান করবে না। সেখানে hash-এর কাজ শুধু "dump পড়ে ID ফিরে পাওয়া যাবে না"। OWASP-ও বলে, এই random মানের জন্য দ্রুত hash যথেষ্ট, password hashing algorithm অপ্রয়োজনীয়। আর দ্রুত বলেই প্রতিটা request-এ খরচ নগণ্য। ঠিক এটাই আমাদের আজকের সমস্যা ছিল।

**একটা সৎ টীকা।** OWASP এও বলে, শক্তভাবে নিয়ন্ত্রিত store-এ আসল token রাখা গ্রহণযোগ্য হতে পারে। hash রাখা দরকার হয় যখন store-এর read-only ফাঁস threat model-এ আছে। OWASP আরেকটা নকশাও দেখায়: cookie-তে দুটো অংশ (একটা খোঁজার identifier, একটা গোপন verifier), store-এ identifier আর verifier-এর hash, আর যাচাইয়ে constant-time তুলনা। আমরা সরল পথে গেছি। এর দাম আছে: খোঁজার সময় যাচাইয়ের অংশ আলাদা নয়। এখানে সেটা শুধু ভাবনা, কোনো প্রমাণিত আক্রমণ নয়। বড় system-এ OWASP-র নকশাটা দেখে নিয়ো।

**login-এর আগে কোনো session নেই।** আমাদের কোডে session তৈরি হয় শুধু password যাচাইয়ের **পরে**। আর `Lookup` শুধু সেই ID চেনে, যা server নিজে বানিয়েছে। এতে অধ্যায় ৫-এর একটা পথ আপাতত বন্ধ। কিন্তু "বন্ধ" বলছি না, "সমস্যা শেষ" বলছি না। fixation পুরোপুরি অধ্যায় ৫-এর বিষয়।

**মেয়াদ server-এর খাতায়।** cookie-তে `MaxAge` নেই। এর কারণ §৮-এ।

### `401`-এর ঋণ শোধ

অধ্যায় ১-এ লিখেছিলাম, `401` response-এ `WWW-Authenticate` header লাগে। অধ্যায় ২-এ বলেছিলাম, browser-এর প্রসঙ্গে সেটা কীভাবে বদলায়, সেটা অধ্যায় ৪-এ। এবার বলি।

RFC 9110 (§15.5.2) অনুযায়ী `401` পাঠালে সেই response-এ অন্তত একটা challenge-সহ `WWW-Authenticate` header পাঠাতে হবে। HTTP-র এই কাঠামো `Basic` (অধ্যায় ১) বা `Bearer`-এর মতো scheme-এর জন্য বানানো, যেখানে client পরের request-এ `Authorization` header জুড়ে দেয়। Basic challenge পেলে browser নিজেই একটা পাসওয়ার্ডের জানালা খোলে, যেটা আমরা চাই না। আর cookie-ভিত্তিক session-এর জন্য এই তালিকায় আমি কোনো scheme খুঁজে পাইনি: IANA-র প্রাথমিক registration-এর তালিকায় Basic, Bearer, Digest আর Negotiate আছে। (পুরো আজকের registry আমি এখানে মেলাইনি।)

তাই cookie-র জগতে সাধারণ চর্চা দুই ভাগে:

- **browser-এর পাতা (GET):** `302` দিয়ে login পাতায় পাঠাই। উপরের কোড তাই করে।
- **অন্য request (যেমন POST বা JavaScript-এর ডাক):** `401`। অনেক framework এটা `WWW-Authenticate` ছাড়াই পাঠায়, যা RFC 9110-এর কঠোর পাঠে অসম্পূর্ণ। আমাদের কোডও তাই করছে। অধ্যায় ৮-এ `Bearer` (RFC 6750) এলে এই API-র `401`-এর একটা প্রমিত challenge পাওয়া যাবে।

এই ছাড় জেনেবুঝে রাখা হলো। নীরবে ধরে নেওয়া হয়নি।

---

## 🛡️ ৭. Cookie-র অ্যাট্রিবিউট: browser-কে দেওয়া নির্দেশ

`setSessionCookie`-এর প্রতিটা লাইন browser-এর জন্য একটা নির্দেশ। তার আগে একটা মূল সত্য, যেটা বুঝলে বাকিটা সহজ হয়ে যায়। browser যখন cookie ফেরত পাঠায়, তাতে থাকে শুধু `নাম=মান`। RFC 6265 স্পষ্ট বলে: `Cookie` header থেকে server জানতে পারে না cookie-র মেয়াদ কবে, কোন host আর path-এর জন্য, বা সেটা `Secure` বা `HttpOnly` ছিল কি না।

```http
Cookie: __Host-id=jd3COo2PDshkWEsI2mswgz39yB6IXw9k1_zW4jdIN2o
```

মানে, অ্যাট্রিবিউটগুলো **কথা দেয় না**। এগুলো একটা ভদ্র browser-কে দেওয়া নির্দেশ। যে `curl` দিয়ে সরাসরি request পাঠায়, তার কাছে `HttpOnly` বা `SameSite`-এর কোনো মানে নেই। এই নির্দেশগুলো রক্ষা করে **browser-এর ভেতরের আর পথের** বিপদ থেকে: অন্য পাতা, অন্য script, নেটওয়ার্ক। যার হাতে মানটা আছে, তাকে এরা থামায় না।

browser আসলে কখন cookie পাঠায়? ছবিটা সরল করে দিলাম, আসল নিয়ম বর্তমান cookie draft-এর retrieval algorithm-এ।

```mermaid
flowchart TD
    R["browser একটা request পাঠাতে যাচ্ছে"] --> H{"cookie-র host মেলে?<br/>Domain না থাকলে ঠিক এই host"}
    H -- "না" --> N["Cookie header-এ যায় না"]
    H -- "হ্যাঁ" --> P{"URL-এর path মেলে?"}
    P -- "না" --> N
    P -- "হ্যাঁ" --> S{"cookie Secure,<br/>কিন্তু সংযোগ HTTPS নয়?"}
    S -- "হ্যাঁ" --> N
    S -- "না" --> C{"request কি cross-site?"}
    C -- "না, same-site" --> Y["Cookie header-এ যায়"]
    C -- "হ্যাঁ" --> L{"SameSite কী?"}
    L -- "None" --> Y
    L -- "Strict" --> N
    L -- "Lax" --> T{"top-level navigation<br/>আর safe method, যেমন GET?"}
    T -- "হ্যাঁ" --> Y
    T -- "না" --> N
```

মেয়াদ-শেষ বা browser-এর মুছে-ফেলা cookie তো আগেই বাদ। আর `HttpOnly` এই ছবিতে নেই, কারণ সেটা **request-এ যাওয়া** নিয়ে নয়, **JavaScript-এর পড়া** নিয়ে। MDN ঠিক এটাই বলে: `HttpOnly` cookie JavaScript থেকে শুরু করা request-এও যায়।

একে একে দেখি, প্রতিটা কী ঠেকায় আর কী ঠেকায় না।

| অ্যাট্রিবিউট | browser-কে কী বলে | কী ঠেকায় | কী ঠেকায় না |
|---|---|---|---|
| `Secure` | শুধু HTTPS-এ পাঠাও | নেটওয়ার্কে আড়ি পেতে cookie পড়া, ভুল করে HTTP-তে যাওয়া | active আক্রমণকারী insecure পথ থেকে cookie **বদলে দেওয়া** (RFC), device থেকে চুরি |
| `HttpOnly` | `document.cookie` থেকে লুকাও | script দিয়ে cookie-র মান **পড়া** | script-এর পাঠানো request (cookie তবু যায়), CSRF, নেটওয়ার্ক, device থেকে চুরি |
| `SameSite` | cross-site request-এ পাঠাবে কি না | কিছু cross-site request-এ cookie যাওয়া | চুরি, same-site আক্রমণ (যেমন সহোদর subdomain), `Lax`-এ top-level GET |
| `Path` | কোন path-এ পাঠাবে | সুবিধা, security নয় | অন্য path থেকে `document.cookie`, অন্য path থেকে cookie বসানো |
| `Domain` | কোন host-এ পাঠাবে | না দিলে cookie শুধু ঠিক ওই host-এ | দিলে সব subdomain, আর তাদের দুর্বলতা |
| `Max-Age`/`Expires` | কতদিন রাখবে | (এটা hint) | server-এর দিক থেকে মেয়াদ, §৮ |
| `__Host-` | browser জোর করে শর্তগুলো মানবে | subdomain বা insecure origin থেকে বসানো/বদলানো | চুরি, XSS, CSRF, port-ভিত্তিক ফাঁক |

### `Secure`: কাফের Wi-Fi

রাফি এক সন্ধ্যায় কোনো কাফের খোলা Wi-Fi থেকে KenaKata-য় ঢুকল। সাইট HTTPS-এই চলে। তবু RFC 6265 এই ঝুঁকির একটা উদাহরণ দেয়। `Secure` না থাকলে active আক্রমণকারী রাফির যে কোনো বাইরের HTTP request ধরে সেটাকে KenaKata-র HTTP ঠিকানায় ঘুরিয়ে দিতে পারে। server সেই পোর্টে না শুনলেও browser cookie জুড়ে পাঠিয়ে দেবে, আর আক্রমণকারী সেটা পড়ে ফেলবে। `Secure` থাকলে browser clear-text request-এ cookie দেয়ই না। OWASP এর সঙ্গে HSTS-ও (HTTP Strict Transport Security) চালু করতে বলে, যেন browser শুরু থেকেই HTTPS ছাড়া যায় না। এখানে HSTS-এর খুঁটিনাটিতে ঢুকছি না।

কিন্তু `Secure`-এর একটা সীমা RFC নিজেই স্বীকার করে: এটা শুধু **গোপনীয়তা** দেয়। active network আক্রমণকারী insecure পথ থেকে একটা cookie বসিয়ে দিতে বা বদলে দিতে পারে, অর্থাৎ **অখণ্ডতা** নেই। `__Host-` prefix এখানে সাহায্য করে: browser সেই cookie শুধু HTTPS origin থেকেই মানে।

### `HttpOnly`: যা লুকোয়, আর যা লুকোয় না

`HttpOnly` cookie-কে `document.cookie` থেকে সরিয়ে রাখে, আর এটা সত্যিই কাজের। কিন্তু একটা ভুল ধারণা এখান থেকেই শুরু: "তাহলে XSS-এ ক্ষতি নেই।" না। পাতায় চলা script cookie **পড়তে** না পারলেও `fetch` দিয়ে request পাঠাতে পারে, আর browser সেই request-এ cookie জুড়ে দেবে। OWASP-র ভাষায়: `HttpOnly` শুধু cookie-র confidentiality রক্ষা করে, আক্রমণকারী সেটা offline ব্যবহার করতে পারে না। অধ্যায় ৭-এ এটা পুরো গল্প।

### `SameSite`: ডিফল্টের ওপর ভরসা নয়

`SameSite` ঠিক করে cross-site request-এ cookie যাবে কি না। তিনটা মান:

- `Strict`: শুধু same-site request-এ যাবে।
- `Lax`: same-site request-এ, আর **cross-site top-level navigation যদি safe method (যেমন GET) হয়**, তাতে। মানে অন্য সাইট থেকে link-এ ক্লিক করলে cookie যায়, কিন্তু অন্য সাইটের form থেকে `POST` গেলে যায় না।
- `None`: সব request-এ, শর্ত `Secure` থাকতে হবে।

আমরা `Lax` বেছেছি, আর স্পষ্ট করে লিখেছি, কারণ আছে। MDN বলে, কিছু browser `SameSite` না থাকলে `Lax` ধরে, কিন্তু সেটা **আরও শিথিল** একটা সংস্করণ: cookie বানানোর দুই মিনিটের মধ্যে cross-site `POST`-এও যেতে পারে। OWASP স্পষ্ট বলে, browser-এর ডিফল্ট মান browser আর version ভেদে আলাদা, তাই ডিফল্টের ওপর ভরসা না করে স্পষ্ট লিখতে।

`Strict` না কেন? OWASP `Strict`-কে পছন্দের বলে, কিন্তু তাতে একটা বাস্তব ঘর্ষণ আছে। অধ্যায় ১-এ আমরা দেখেছিলাম, কর্মীরা link শেয়ার করে চ্যাটে। কেউ যদি ওয়েব-ভিত্তিক email বা chat থেকে link ধরে `/manage/orders`-এ আসে, `Strict` cookie সেই প্রথম navigation-এ যাবে না। session ঠিক থাকলেও পাতা তাকে login-এ পাঠিয়ে দেবে, আর দ্বিতীয় ক্লিকে আবার কাজ করবে। রাফির কর্মীদের জন্য এটা বিরক্তিকর, তাই এই দৃশ্যে `Lax`। (native app থেকে খোলা link-এর আচরণ browser ভেদে ভিন্ন, নিজে পরীক্ষা করে নিয়ো।)

আর `SameSite` CSRF-এর সমাধান নয়। cookie draft নিজেই বলে, `Lax` CSRF-এর বিরুদ্ধে defense in depth, কিন্তু "সাধারণ আক্রমণ-শ্রেণি"র বিরুদ্ধে শক্ত সুরক্ষা নয়। আক্রমণকারী নতুন window খুলে top-level navigation ঘটাতে পারে। অধ্যায় ৬।

একটা নামের ফাঁদ: **same-site** আর **same-origin** এক নয়। cookie draft অনুযায়ী site মানে scheme আর registrable domain। `a.kenakata.example` আর `b.kenakata.example` same-site, কিন্তু আলাদা origin। `Domain` প্রসঙ্গে এটা কাজে লাগবে।

### `Path` আর `Domain`: যত সংকীর্ণ, তত ভালো

`Path` দিয়ে cookie শুধু `/manage`-এ পাঠাতে চাইলে চাইতে পারো, কিন্তু MDN সাফ বলে, `Path` security-র ব্যবস্থা নয়। অন্য path-এর পাতার script `document.cookie` দিয়ে পড়তে পারে, আর যে কোনো পাতা যে কোনো path-এর cookie বসাতে পারে (RFC 6265 §৮.৫, §৮.৬)। তাই `Path=/`।

`Domain` না দিলে cookie শুধু যে host দিয়েছে সেই host-এ যায় (host-only)। `Domain=kenakata.example` দিলে সব subdomain-এ যায়, আর সহোদর subdomain-গুলো cookie বদলে দিতে পারে। ধরো (কাল্পনিক, KenaKata-র আসল নকশা নয়) কাল কেউ `blog.kenakata.example`-এ একটা পুরোনো CMS বসাল, আর তাতে দুর্বলতা আছে। `Domain` দিলে সেই দুর্বলতা মূল সাইটের session-কে ছুঁয়ে ফেলতে পারে। OWASP-ও বলে `Domain` না দিতে।

### `__Host-` prefix: browser-কে দিয়ে শর্ত মানানো

নামের শুরুতে `__Host-` থাকলে browser শুধু সেই cookie গ্রহণ করে যার `Secure` আছে, `Domain` নেই, আর `Path=/`। অর্থাৎ cookie শুধু ঠিক এই host-এর, সহোদর subdomain বা insecure পথ বদলাতে পারবে না। OWASP session ID-র জন্য এটা সুপারিশ করে। সীমা দুটো। এক, port আলাদা করা হয় না (RFC 6265 §৮.৫ মতে একই host-এর সব port cookie ভাগ করে, আর draft বলে `__Host-` এই একটা ফাঁকই রেখে যায়)। দুই, যে browser prefix বোঝে না, সে cookie-টা তবু গ্রহণ করবে, কোনো শর্ত ছাড়াই (MDN-এর সতর্কতা)।

লোকাল development-এ MDN বলে `localhost`-এ `Secure`-এর https শর্ত শিথিল হয়। তবে `__Host-` prefix সব browser-এ `localhost`-এ একই আচরণ করবে, এমন ধরে নিয়ো না, নিজে পরীক্ষা করো।

---

## ⏳ ৮. মেয়াদ: দুটো ঘড়ি, আর শুধু একটা সত্যি

cookie-র মেয়াদ আর session-এর মেয়াদ আলাদা জিনিস, আর দুটোর ওজনও আলাদা।

**cookie-র মেয়াদ browser-এর ব্যাপার।** `Expires` বা `Max-Age` দিলে cookie persistent হয়। না দিলে সেটা **session cookie**: browser বন্ধ হলে মুছে যাওয়ার কথা। কিন্তু এর সঙ্গে কয়েকটা সতর্কতা:

- browser যে কোনো সময় cookie মুছে ফেলতে পারে। RFC 6265-এর ভাষা: user agent মেয়াদ পর্যন্ত রাখতে বাধ্য নয়, আর server যেন cookie থেকে যাবে ধরে না নেয়।
- `Expires` server-এর ঘড়ির সময়ে লেখা হয়, কিন্তু user agent নিজের ঘড়িতে মাপে। MDN বলে, Firefox আর Chromium-ভিত্তিক browser `Date` header দেখে এই ফারাক সামলায়, কিন্তু spec সেটা বাধ্যতামূলক করে না।
- draft বলে, user agent cookie-র মেয়াদ সর্বোচ্চ ৪০০ দিনে কেটে দেবে (SHOULD NOT তার বেশি)। তুমি দশ বছর লিখলেও।
- "browser বন্ধ করলে session cookie মুছে যায়" এর ওপরও ভরসা নেই। browser-এর session restore থাকলে session cookie ফিরে আসে (MDN আর OWASP দুজনেই এই সতর্কতা দেয়)।

**session-এর মেয়াদ server-এর খাতার ব্যাপার, আর সেটাই আসল।** আমাদের `Lookup` প্রতিটা request-এ `ExpiresAt` দেখে। cookie যতই বলুক, খাতায় মেয়াদ শেষ মানে শেষ। OWASP-ও বলে, session-এর timeout server-এ প্রয়োগ করতে হবে, কারণ client-এর ওপর নির্ভর করলে আক্রমণকারী সময় বদলে দিতে পারে।

আরিফ তাই বেছে নিল: cookie-তে মেয়াদ নেই (session cookie), আর server-এ ৮ ঘণ্টা। কেন ৮? OWASP-র উদাহরণ: যে application একজন office কর্মী পুরো দিন ব্যবহার করে, তার absolute timeout ৪ থেকে ৮ ঘণ্টার মধ্যে হতে পারে। দোকানের একটা কর্মদিবসও মোটামুটি তাই। কেন persistent নয়? অধ্যায় ১-এ দোকানের shared কম্পিউটারের history আর autocomplete-এর কথা বলেছিলাম। কিছু কম্পিউটারে কেউ লগআউট না করেই উঠে যায়।

এর দাম আছে, আর নীরা সেটা প্রথম দিনই টের পাবে। রাফিকে প্রতিদিন সকালে login করতে হবে। "আমাকে মনে রাখো" ধরনের দীর্ঘ session মানে চুরি-যাওয়া cookie-র আয়ু বাড়া। এটা একটা ব্যবসার সিদ্ধান্ত, শুধু প্রযুক্তির নয়। আর ৮ ঘণ্টাকে **idle** আর **absolute** এই দুই ঘড়িতে ভাগ করা অধ্যায় ৫-এর কাজ।

এই ভাগের ফলে একটা গুরুত্বপূর্ণ কথা, যা OWASP স্পষ্ট বলে: **browser থেকে cookie মুছে ফেলা session বাতিল করে না।** মুছলে শুধু ওই browser-এর কপিটা যায়। চুরি-যাওয়া কপি কাজ করতেই থাকে, যতক্ষণ না server খাতা থেকে লাইনটা কাটে।

---

## 🔍 ৯. বাতিল: খাতা থেকে লাইন কাটা

বুধবার। রাফি জানাল, কর্মী `user-7`-কে আজ থেকে ছাড়িয়ে দিয়েছে। কিন্তু ফোনের browser-এ তার login এখনো খোলা। অধ্যায় ২ থেকে এই প্রশ্নটা ঝুলছিল।

আরিফ দুটো ফাংশন যোগ করল:

```go
// Delete: একটা session বাতিল (logout)।
func (s *Store) Delete(token string) {
	s.mu.Lock()
	delete(s.rows, digest(token))
	s.mu.Unlock()
}

// DeleteAllFor: একজন user-এর সব session বাতিল।
func (s *Store) DeleteAllFor(userID string) int {
	s.mu.Lock()
	defer s.mu.Unlock()
	n := 0
	for k, r := range s.rows {
		if r.UserID == userID {
			delete(s.rows, k)
			n++
		}
	}
	return n
}
```

`sessions.DeleteAllFor("user-7")` চালানো মাত্র `user-7`-এর পরের প্রতিটা request-এ `Lookup` ব্যর্থ হয়, আর সে login পাতায় ফেরে। সই-করা cookie দিয়ে এটা হতো না, §৩-এর দ্বিতীয় প্রশ্ন।

আর logout:

```go
func logoutHandler(w http.ResponseWriter, r *http.Request) {
	if c, err := r.Cookie(cookieName); err == nil {
		sessions.Delete(c.Value) // আগে server-এর খাতা: এটাই আসল বাতিল
	}
	clearSessionCookie(w) // তারপর browser-এর cookie: শুধু পরিচ্ছন্নতা
	http.Redirect(w, r, "/login", http.StatusSeeOther)
}
```

`clearSessionCookie` পাঠায় `__Host-id=; Path=/; Max-Age=0; HttpOnly; Secure; SameSite=Lax` (Go-তে `MaxAge: -1` এই header হয়, আমি দেখেছি)। RFC 6265 বলে, cookie মুছতে চাইলে `Path` আর `Domain` অবশ্যই cookie বানানোর সময়ের সঙ্গে মিলতে হবে। আমাদের `Path=/`, `Domain` নেই, তাই মেলে।

আমার test-এ একটা জিনিস পরীক্ষা করেছি: login করে cookie-র মান আলাদা করে রাখলাম, logout করলাম, তারপর সেই রাখা মান `curl`-এর মতো করে আবার পাঠালাম। server ফিরিয়ে দিল। অর্থাৎ "আগে server, পরে browser" এই ক্রমটা সত্যিই কাজ করছে। উল্টো করলে (শুধু cookie মুছলে) চুরি-যাওয়া কপি বেঁচে থাকত।

logout একটা `POST`, `GET` নয়। কেন, সেটা অধ্যায় ৬-এ আসবে। আর "logout" বলতে আসলে কী বোঝাচ্ছি (একটা device, সব device, নাকি চুরি-যাওয়া credential), সেই পুরো প্রশ্ন অধ্যায় ১৪-এর। এখানে শুধু প্রথম টুকরো: session-এ বাতিলের হাতলটা আছে।

---

## ১০. Threat model: session ID-র পাশে আক্রমণকারী

ছবিটা আগে। প্রশ্ন: session ID বা তার ছাপ কোথায় কোথায় পৌঁছায়, আর কোথায় সেটা আসল রূপে?

```mermaid
flowchart LR
    subgraph Device["কর্মীর device"]
        Jar["Cookie jar<br/>আসল session ID"]
        Page["পাতার JavaScript"]
    end
    subgraph Net["পথ"]
        Wire["নেটওয়ার্ক<br/>HTTPS না হলে আসল ID"]
    end
    subgraph Srv["KenaKata server"]
        Mem["request চলার সময় memory<br/>আসল ID, কয়েক ms"]
        Log["log, trace, proxy<br/>সাবধান না হলে আসল ID"]
        Store[("Session store<br/>শুধু hash")]
    end
    Jar -->|"Cookie header"| Wire
    Wire --> Mem
    Mem --> Log
    Mem -->|"hash করে খোঁজা"| Store
    Page -.->|"HttpOnly না থাকলে পড়তে পারে"| Jar
```

আসল ID থাকে চার জায়গায়: cookie jar, পথে (TLS ছাড়া), server-এর memory-তে কয়েক মিলিসেকেন্ড, আর যদি আমরা অসাবধান হই তাহলে log বা tracing-এ। store-এ থাকে না। OWASP-র নির্দেশ: log-এ আসল session ID নয়, দরকার হলে salt-যুক্ত hash লেখো, যাতে একটা session-এর ঘটনা মেলানো যায় কিন্তু ID ফাঁস না হয়।

এখন আক্রমণকারীর প্রশ্নগুলো:

| প্রশ্ন | উত্তর |
|---|---|
| কী control করে? | নিজের browser আর নিজের পাঠানো যে কোনো `Cookie` মান। দূষিত পাতা যা browser-কে দিয়ে request করাতে পারে (অধ্যায় ৬)। আমাদের পাতায় চলা script (অধ্যায় ৭)। সহোদর subdomain (যদি `Domain` দেওয়া হয়)। insecure পথে active নেটওয়ার্ক আক্রমণকারী |
| কী steal করতে পারে? | cookie jar থেকে (shared কম্পিউটার, malware, `HttpOnly` না থাকলে script)। নেটওয়ার্ক থেকে (TLS বা `Secure` না থাকলে)। log থেকে (যদি আসল ID লেখা হয়)। store থেকে শুধু hash, যা দিয়ে cookie বানানো যায় না |
| কী observe করতে পারে? | TLS ছাড়া `Cookie` আর `Set-Cookie` পুরোটা। রাফির নিজের browser-এর developer tools-এ রাফি নিজেই সব দেখতে পায় |
| কী modify করতে পারে? | নিজের cookie-র মান যা খুশি করা। server অচেনা ID ফিরিয়ে দেয়। active নেটওয়ার্ক বা সহোদর subdomain নিজের cookie বসিয়ে দিতে পারে (`__Host-` ও `Secure` এটা কঠিন করে) |
| কী replay করতে পারে? | **হ্যাঁ, session ID-র মান পেলে যেখান থেকে খুশি।** server সামনের request দুটোর মধ্যে তফাত দেখে না, কারণ দুটোতেই একই মান |
| ফাঁস হলে কী হবে? | আক্রমণকারী ওই user-এর পরিচয়ে কাজ করে। ক্ষতি সেই user-এর ক্ষমতার সমান (অধ্যায় ২)। তবে password পায় না, তাই অন্য সাইটে reuse-এর ঝুঁকি নেই |
| কতক্ষণ access? | সর্বোচ্চ server-এর খাতার মেয়াদ (এখানে ৮ ঘণ্টা), অথবা বাতিলের আগ পর্যন্ত |
| server কি ধরতে পারবে? | কঠিন। বৈধ ID দিয়ে বৈধ-দেখতে request। OWASP IP বা User-Agent-এর সঙ্গে ID বেঁধে সন্দেহজনক পরিবর্তন ধরতে বলে, কিন্তু সেগুলো একই network-এ থাকা আক্রমণকারী পার করে ফেলতে পারে। অধ্যায় ৫ |
| server কি revoke করতে পারবে? | হ্যাঁ, একটা row মুছে। একজনের জন্য বা সবার জন্য |
| ক্ষতি কীভাবে সীমিত হয়? | ছোট মেয়াদ, `Secure` আর TLS, `HttpOnly`, `__Host-`, store-এ hash, log-এ আসল ID নয়, least privilege (অধ্যায় ২), সংবেদনশীল কাজে আরেক ধাপ (অধ্যায় ২২) |

---

## ১১. আমরা কি ঠিক সেই জায়গায় ফিরলাম?

অধ্যায় ৩-এর শেষ প্রশ্নটা ছিল: যে চিহ্ন প্রতিটা request-এ যায়, সেটা চুরি হলে আমরা কি আবার সেখানেই, যেখান থেকে বেরিয়ে এলাম?

উত্তর: **একই ধরনের বিপদ, ছোট আকারে।** OWASP-র কথায়, authenticated session স্থাপিত হলে session ID সাময়িকভাবে ব্যবহৃত সবচেয়ে শক্ত authentication পদ্ধতির সমতুল্য। মানে বিপদটা নেই বলছি না। কিন্তু পাশাপাশি রাখলে তফাত পরিষ্কার:

| | password | session ID |
|---|---|---|
| চুরি হলে অন্য সাইটে ব্যবহার? | হ্যাঁ, reuse থাকলে | না, প্রতিটা login-এ নতুন, random |
| কে বানায়? | user, অনুমানযোগ্য | server, অনুমান-অযোগ্য |
| কতদিন বৈধ? | বদলানো পর্যন্ত, মাস-বছর | ঘণ্টা, আর বাতিলযোগ্য |
| বাতিল করতে? | user-কে নতুন password বানাতে হয় | server একটা row কাটে |
| প্রতি request-এ খরচ? | ভারী (Argon2id) | হালকা (একটা খোঁজ) |
| **যার হাতে, সে-ই?** | **হ্যাঁ** | **হ্যাঁ** |

শেষ সারিটাই এই অধ্যায়ের বিনয়ী উপসংহার। আমরা bearer সমস্যা সমাধান করিনি। আমরা তার **আকার** ছোট করেছি: সীমিত মেয়াদ, বাতিলযোগ্য, আর password-এর সঙ্গে বাঁধা নয়।

---

## ১২. Trade-off: state-এর দাম

- **server-কে মনে রাখতে হয়।** প্রতিটা request-এ একটা খোঁজ। store বন্ধ হলে সবাই login থেকে বঞ্চিত। একাধিক server চালালে সবাইকে একই store দেখতে হয় (database বা Redis-এর মতো কিছু), নইলে একটা server-এর বানানো session আরেকটা চেনে না।
- **store নিজেই একটা লক্ষ্য।** hash রাখলে read-only ফাঁসে ক্ষতি কমে, কিন্তু store-এ লেখার সুযোগ পেলে আক্রমণকারী নিজের সারি বসিয়ে দিতে পারে।
- **পুরোনো row জমে।** আমাদের কোড শুধু `Lookup`-এ মেয়াদ-শেষ row সরায়। production-এ আলাদা পরিষ্কারের কাজ (TTL বা সময় ধরে মোছা) লাগে।
- **`SameSite=Lax`-এর ছাড়।** `Strict` শক্ত কিন্তু link ধরে আসা কর্মীদের ঘর্ষণ। `Lax` সহজ কিন্তু CSRF-এর পুরো সমাধান নয়।
- **৮ ঘণ্টার মেয়াদ।** ছোট মেয়াদ নিরাপদ, কিন্তু রাফিকে রোজ সকালে login। দীর্ঘ মেয়াদ সুবিধাজনক, কিন্তু চুরির সুযোগ বড়।
- **cookie শুধু browser-এর জিনিস।** browser নয় এমন client-এ cookie অস্বস্তিকর। সেই চাপ আসবে অধ্যায় ৮-এ।
- **একসঙ্গে অনেক device।** প্রতিটা login-এর নিজস্ব session। সুবিধা: "আমার চালু device-এর তালিকা" দেখানো যায়। দাম: তালিকা সামলাতে হয়।
- **ছোট জিনিস, যা কোডে রাখিনি।** `Clear-Site-Data` (logout-এ browser-এর cache আর cookie মুছতে বলা), HSTS, একজনের সর্বোচ্চ কয়টা session, ব্যর্থ ID-র অনুমান শনাক্ত করা, আর session ঘটনার log। OWASP-র পাতায় সব আছে।

---

## 🛑 ১৩. এই সমাধান কী সমাধান করে না

- **session ID চুরি।** cookie যার হাতে, সে-ই। শুধু দাম কমেছে (§১১)। অধ্যায় ৫-এর পুরো প্রশ্ন।
- **session fixation।** আমরা login-এর আগে session বানাই না, কিন্তু আক্রমণকারী আগেই ID বসিয়ে দেওয়ার পথ, login-এর সময় ID বদলানো, আর অন্য নকশা, এগুলো অধ্যায় ৫।
- **CSRF।** cookie ambient authority। অন্য সাইটের request-এ browser cookie জুড়ে দিতে পারে। `SameSite` আংশিক সাহায্য করে। অধ্যায় ৬।
- **XSS।** পাতায় চলা script request করাতে পারে। `HttpOnly` কেবল cookie-র মান পড়া আটকায়। অধ্যায় ৭।
- **দুর্বল বা ফিশ করা password।** session শুরু হয় password দিয়ে। পরিচয় ভুল হলে session ভুল মানুষের হাতেও নিঃশব্দে তৈরি হবে (অধ্যায় ৩, ২২)।
- **authorization।** session বলে "এ user-1"। user-1 কী পারে, সেটা অধ্যায় ২ আর ১৫।
- **server বা store-এ ঢোকা আক্রমণকারী।** আসল ID request চলার সময় memory-তে আছে, আর store-এ লিখতে পারলে নিজের session বানাতে পারে।
- **idle আর absolute আলাদা timeout, session-এর মধ্যে সুরক্ষা বাড়ানো।** অধ্যায় ৫।
- **logout-এর পুরো মানে।** আজ শুধু বাতিলের হাতল। পুরো প্রশ্ন অধ্যায় ১৪।
- **browser নয় এমন client।** অধ্যায় ৮।

---

## ১৪. সাধারণ ভুল ধারণা

**"Cookie মানেই authentication।"** cookie একটা বাহক। কী বইছে, তার ওপর সবকিছু।

**"Cookie আর session একই জিনিস।"** cookie browser-এর মেমোরি, session server-এর রেকর্ড। cookie-তে session ID থাকতে পারে, ভাষার পছন্দও থাকতে পারে।

**"cookie encrypt করলে চুরি-নিরাপদ।"** RFC 6265: encrypt বা sign করলেও cookie এক জায়গা থেকে আরেক জায়গায় বসানো বা পরে আবার চালানো যায়। session ID নিজেই credential।

**"`HttpOnly` আর `Secure` দিলে session চুরি অসম্ভব।"** shared কম্পিউটার, malware, log, XSS-এর করা request, এগুলো এই দুটোর বাইরে।

**"`Path=/manage` দিয়ে আলাদা করলাম, তাই নিরাপদ।"** MDN আর RFC দুজনেই বলে, `Path` security-র ব্যবস্থা নয়।

**"browser বন্ধ করলে session শেষ।"** session restore cookie ফেরায়। আর server-এর খাতায় session বেঁচে থাকে।

**"cookie মুছলাম, তাই logout।"** শুধু ওই browser-এর কপি গেছে। চুরি-যাওয়া কপি চলে যতক্ষণ খাতায় লাইন আছে।

**"cookie-তে user ID রাখলাম, এটাই session।"** §২। ID একটা দাবি।

**"`SameSite=Lax` দিলেই CSRF শেষ।"** draft-এর নিজের ভাষায় এটা defense in depth, শক্ত সমাধান নয়।

**"session পুরোনো ধারণা, তাই token নিলেই ভালো।"** কোনোটাই সর্বজনীনভাবে ভালো নয়। session আর token একই জিনিস বহন করে ভিন্ন ভিন্ন দামে। অধ্যায় ৮ থেকে ১০ পাশাপাশি রাখবে।

**"UUID মানেই নিরাপদ session ID।"** UUID কোন version, কোন random উৎস থেকে, না জেনে ভরসা নয়। আমরা সরাসরি `crypto/rand` ব্যবহার করেছি, যেটা ঠিক জানি।

---

## ১৫. এক নজরে: Session আর Cookie

| প্রশ্ন | উত্তর |
|---|---|
| এটা কী? | **Session:** login-এর পর server-এর রাখা রেকর্ড। **Session ID:** তার চাবি। **Cookie:** ID বইবার browser-এর কৌশল |
| কেন এসেছে? | প্রমাণিত identity প্রতিটা request-এ হালকা ভাবে বহন করতে, password ঘোরানো ছাড়া |
| কী সমাধান করে? | প্রতি request-এ password না পাঠানো, জালিয়াতি (অনুমান-অযোগ্য ID), একজনকে আলাদা বাতিল, password থেকে আলাদা করা |
| কী সমাধান করে না? | চুরি, fixation, CSRF, XSS, দুর্বল password, authorization, server-এ ঢোকা আক্রমণকারী (§১৩) |
| কোথায় থাকে? | cookie jar-এ আসল ID। server-এর খাতায় hash আর রেকর্ড |
| কে ব্যবহার করে? | browser পাঠায়। server খোঁজে। JavaScript সাধারণত পায় না (`HttpOnly`) |
| কীভাবে abuse হয়? | চুরি, আগে-থেকে-বসানো ID, ambient authority, অন্য পাতার script, log-এ ফাঁস |
| ক্ষতি? | ওই user-এর ক্ষমতা, মেয়াদ বা বাতিল পর্যন্ত। password ফাঁস হয় না |
| মেয়াদ? | server-এর খাতায় (এখানে ৮ ঘণ্টা)। cookie-র মেয়াদ শুধু hint |
| বাতিল? | খাতা থেকে row মোছা। একজন বা সবাই |
| বিকল্প? | সই-করা cookie (client-এ state), token (অধ্যায় ৮), HTTP Basic (অধ্যায় ১-এর ব্যর্থতা) |
| কখন লাগে? | server-rendered web application, যেখানে browser-ই client আর বাতিল দরকার |
| কখন লাগে না? | যেখানে browser নেই, বা server কোনো state রাখতে পারে না বা চায় না |
| Production trade-off? | store-এর নির্ভরতা আর scaling, মেয়াদ বনাম ঘর্ষণ, `Lax` বনাম `Strict`, cookie শুধু browser-এ (§১২) |

---

## আমরা কী শিখলাম? (What did we learn?)

- password প্রমাণ করে, session প্রমাণিত অবস্থা ধরে রাখে। একটা ভারী আর একবার, অন্যটা হালকা আর প্রতিবার।
- cookie নিজে authentication নয়, একটা বাহক। কী বইছে, সেটাই আসল প্রশ্ন।
- ID দিয়ে "আমি কে" বোঝানো যায় না, কারণ ID জানা সহজ। দরকার এমন একটা মান, যা অনুমান করা যায় না আর যার অর্থ শুধু server-এর খাতায়।
- সই-করা cookie আর server-side session দুটো আলাদা trade-off: client-এ state বনাম বাতিলের ক্ষমতা। একটা ভুল, অন্যটা ঠিক, এভাবে নয়।
- session ID চুরি আমরা আটকাইনি। চুরির পরের অবস্থা বদলেছে: সীমিত মেয়াদ, বাতিলযোগ্য, password আলাদা।
- store-এ ID-র hash রাখি, কারণ read-only ফাঁসে ID দিয়ে session দখল ঠেকাতে। random মানের জন্য SHA-256 যথেষ্ট, Argon2id নয়।
- cookie-র অ্যাট্রিবিউট browser-কে দেওয়া নির্দেশ। server `Cookie` header-এ সেগুলো দেখে না। যার হাতে মান আছে, তাকে এরা থামায় না।
- `HttpOnly` পড়া আটকায়, ব্যবহার আটকায় না। `SameSite=Lax` CSRF-এর শক্ত সমাধান নয়। `Path` security নয়।
- cookie-র মেয়াদ browser-এর হাতে। আসল মেয়াদ থাকে server-এ। browser থেকে cookie মুছলে session বাতিল হয় না।

## 🎯 নিজের বোঝাপড়া যাচাই

রাফি browser থেকে logout করল। browser cookie মুছে ফেলেছে, কিন্তু server-এর session store-এ record এখনো আছে। কেউ যদি logout-এর আগে কপি করা session ID দিয়ে request পাঠায়, কী হবে?

<details>
<summary>উত্তর ও ব্যাখ্যা দেখো</summary>

যদি server-side session record এখনো বৈধ থাকে, request সফল হতে পারে। Browser-এর cookie মুছে ফেলা শুধু ওই browser-এর কপিটা সরায়, server-এর record বাতিল করে না। সঠিক logout-এ server-side session-ও invalidate করতে হবে, তারপর browser-কে cookie মুছতে নির্দেশ দিতে হবে।

</details>

## 🧪 পাঁচ মিনিটের পরীক্ষা

নিজের কোনো project-এর **staging**-এ (production-এ নয়):

**এক।** browser-এর developer tools-এ cookie-টা খোলো। `Secure`, `HttpOnly`, `SameSite`, `Path`, `Domain`, মেয়াদ, সব ঠিকঠাক আছে? session ID-র নাম কি framework-এর ডিফল্ট (`PHPSESSID`, `JSESSIONID`)?

**দুই।** logout-এর আগে cookie-র মান কপি করে রাখো। browser-এ logout করো। তারপর:

```bash
curl -i -H 'Cookie: <cookie-র নাম>=<কপি-করা মান>' https://staging.example/protected
```

`200` এলে তোমার logout শুধু cookie মুছেছে, session নয়।

**তিন।** session store-এর টেবিল খোলো (শুধু staging-এ)। cookie-র মানটা সেখানে হুবহু পাওয়া যায়? পেলে store-এর ফাঁস মানে সব চালু session-এর ফাঁস।

**চার।** application log-এ `grep` করো, আসল session ID কোথাও লেখা হচ্ছে কি না।

## একটা অস্বস্তিকর প্রশ্ন (One uncomfortable question)

আজ আমরা দেখলাম, session ID চুরি হলে ক্ষতি password-এর চেয়ে ছোট। কিন্তু ছোট মানে শূন্য নয়। ধরো কেউ রাফির cookie-টা পেয়ে গেল: কাফের Wi-Fi থেকে, shared কম্পিউটারের browser থেকে, কোনো ভুলে-লেখা log থেকে। server-এর চোখে এখন দুটো request হুবহু এক: একটা রাফির নিজের, একটা আক্রমণকারীর। একই cookie, একই মান।

**server কীভাবে বুঝবে কোনটা আসল রাফি? আর আরও খারাপ: আক্রমণকারী যদি রাফির cookie চুরিই না করে, বরং নিজেই একটা session ID বানিয়ে রাফিকে দিয়ে সেটা দিয়ে login করিয়ে নেয়?**

---

## পরের অধ্যায়

[অধ্যায় ৫: Session Hijacking এবং Session Fixation](../05-session-hijacking-fixation/README.md)। আক্রমণকারী রাফির session ID পেয়ে যায়। আর তার আরেক ভাই: login-এর আগে থেকেই session ID জানা। সেখানে আমরা idle আর absolute timeout আলাদা করব, login-এর সময় ID বদলাব, আর দেখব এই সুরক্ষাগুলোরও কোথায় সীমা।

---

## তথ্যসূত্র

- [RFC 6265: HTTP State Management Mechanism](https://www.rfc-editor.org/rfc/rfc6265): প্রকাশিত cookie RFC। পুরো পাঠ পড়ে মেলানো হয়েছে: §১ (Secure-এর অখণ্ডতার সীমা, port-এর আলাদা না হওয়া), §৩.১, §৪.১.২ (Expires, Max-Age, Domain, Path, Secure, HttpOnly), §৪.২.২ (Cookie header-এ অ্যাট্রিবিউট নেই), §৫.৩, §৬.১ (user agent যে কোনো cookie মুছতে পারে), §৮.১–৮.৬ (ambient authority, clear text, session identifier, session fixation, weak confidentiality, weak integrity)।
- [draft-ietf-httpbis-rfc6265bis-22](https://datatracker.ietf.org/doc/draft-ietf-httpbis-rfc6265bis/): এই লেখার সময় datatracker-এ Active Internet-Draft, IESG state "RFC Ed Queue", RFC Editor status "In Final Review"। এখনো RFC নয়, তাই draft হিসেবেই উদ্ধৃত। Internet-Draft নিজেই বলে, একে work in progress ছাড়া অন্যভাবে উদ্ধৃত করা অনুচিত। কেন্দ্রীয় অংশ পড়ে মেলানো হয়েছে: §৪.১.২.৭, §৪.১.৩ (`__Secure-`, `__Host-`), §৫.৫ (৪০০ দিনের সীমা), §৫.৬.৭ (SameSite: Strict, Lax, Lax-allowing-unsafe), §৫.৭ (storage model), §৫.৮.৩ (retrieval algorithm)। §৮-এর SameSite-সংক্রান্ত অংশ (§৮.৮) পুরো পড়া হয়নি, তাই সেখানকার কোনো দাবি এখানে নেই। যেহেতু draft বদলাতে পারে, লেখার আগে আবার দেখে নিয়ো। RFC হয়ে গেলে লিংক বদলাবে।
- [MDN: Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie): পাতাটা ২০২৬-এর ১ সেপ্টেম্বর সর্বশেষ বদলানো। পুরোটা পড়া হয়েছে: SameSite-এর ডিফল্ট আর দুই মিনিটের ছাড়, HttpOnly cookie JavaScript-এর request-এ যাওয়া, Path "security measure নয়", localhost-এ Secure, prefix, session restore আর Expires-এর ঘড়ির ফারাক।
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html): এই লেখার সময় পুরো পড়া হয়েছে। session ID-র entropy (কমপক্ষে ৬৪ bit, নিজে বানালে CSPRNG-এর ১২৮ bit), generic নাম, server-side storage আর hash-এর বিকল্প, সরল hash যথেষ্ট, strict mode, URL-এর বদলে cookie, `__Host-`, SameSite স্পষ্ট লেখা, মেয়াদ server-এ প্রয়োগ, cookie মুছলে session বাতিল হয় না, `Cache-Control: no-store`, log-এ আসল ID নয়, IP বা User-Agent-এর সঙ্গে binding-এর সীমা।
- [RFC 9110: HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110): §১১.৬.১ (`WWW-Authenticate`) ও §১৫.৫.২ (`401 Unauthorized`) মূল RFC Editor-এর পাঠ থেকে পড়ে মেলানো হয়েছে। 401 response-এ challenge থাকা দরকার; cookie-session উদাহরণে এই বিষয়টি এখনো একটি নথিভুক্ত standards caveat, কারণ প্রচলিত cookie login-এর জন্য উপযুক্ত challenge scheme নির্বাচন করা হয়নি।
- IANA HTTP Authentication Scheme Registry: initial registration-এর draft (draft-ietf-httpbis-authscheme-registrations) থেকে দেখা হয়েছে যে Basic, Bearer, Digest, Negotiate নথিভুক্ত। আজকের পুরো registry এখানে মেলানো হয়নি।
- [Wikipedia: HTTP cookie](https://en.wikipedia.org/wiki/HTTP_cookie): শুধু ১৯৯৪-এর ইতিহাসের জন্য (Montulli, MCI, shopping cart)। এটা একটা secondary source, কোনো security দাবির ভিত্তি নয়।
- Go: এই অধ্যায়ের কোড Go 1.22.2 দিয়ে compile করে ও `go test` চালিয়ে দেখা হয়েছে। test পরীক্ষা করেছে: login ছাড়া `GET` redirect হয়, `POST` `401` পায়, ভুল password প্রত্যাখ্যান, সফল login-এ `Set-Cookie` header-এর রূপ, store-এ আসল ID নেই আর store-এর hash cookie হিসেবে পাঠালে প্রত্যাখ্যান, বদলে-দেওয়া ID আর `user=user-1`-এর মতো cookie প্রত্যাখ্যান, logout-এর পর সংরক্ষিত cookie replay প্রত্যাখ্যান, মেয়াদের এক সেকেন্ড আগে-পরে, `DeleteAllFor`। কয়েকটা জিনিস চালানো হয়নি: §২-এর প্রথম ভাবনার কোড, বাস্তব browser-এ `SameSite` আর `__Host-`-এর আচরণ, একাধিক server।

## লেখার অবস্থা

- [x] Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published