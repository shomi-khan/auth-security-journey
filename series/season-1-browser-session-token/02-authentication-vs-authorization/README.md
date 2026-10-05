# Authentication বনাম Authorization

> Login করা আর কোনো কাজের অনুমতি পাওয়া এক জিনিস নয়। এই অধ্যায়ে KenaKata-র প্রথম দোকান দিয়ে এই দুটো প্রশ্ন আলাদা করব।

## রাফির দোকানে নতুন একজন

আরিফের login এখন চলছে। কীভাবে চলছে, সেটা আমরা তৃতীয় আর চতুর্থ অধ্যায়ে দেখব। এই অধ্যায়ের জন্য শুধু একটা জিনিস ধরে নাও: server প্রতিটা request-এ জানে সেটা কোনো চেনা মানুষের, নাকি অচেনা কারও।

KenaKata তখনো একটাই দোকান: রাফির পোশাকের দোকান। Login করে মালিক আর কর্মী। ক্রেতা login করে না।

এক সন্ধ্যায় নীরা একটা message পাঠাল।

> রাফি কাল থেকে এক কর্মী রাখছে। সে order দেখবে, order সামলাবে। কিন্তু টাকা ফেরত, মানে refund, শুধু রাফি দেবে।

আরিফ একটু ভাবল। তারপর নিজেকেই বলল, "Login তো আছেই। যে ঢুকতে পারছে, সে তো রাফির লোক।"

## আরিফের প্রথম চেষ্টা

Refund-এর handler আগে থেকেই ছিল। আরিফ শুধু দেখে নিল, request-টা কোনো চেনা user-এর কি না।

```go
func refundHandler(w http.ResponseWriter, r *http.Request) {
    user := currentUser(r) // কে এই request পাঠিয়েছে? (অধ্যায় ৩–৪)
    if user == nil {
        http.Error(w, "আগে login করো", http.StatusUnauthorized)
        return
    }

    // Login আছে। তাহলে refund হোক।
    refund(r.PathValue("id")) // refund() এখানে কাল্পনিক
    w.WriteHeader(http.StatusNoContent)
}
```

> এই অধ্যায়ের code-এ `currentUser` আর `refund` কাল্পনিক। `currentUser` কীভাবে user চেনে, সেটা পরের অধ্যায়ের গল্প। এখানে শুধু দেখছি, চেনার পরে server কী প্রশ্ন করে।

পরদিন নীরা আরেকটা কথা তুলল, "কর্মীর স্ক্রিনে refund button থাকাটাই তো ঠিক না।"

আরিফ template বদলে দিল।

```html
{{if .User.IsOwner}}
  <form method="post" action="/orders/{{.Order.ID}}/refund">
    <button>Refund করো</button>
  </form>
{{end}}
```

কর্মী এখন আর button দেখে না। আরিফ খুশি। নীরাও।

## দুই সপ্তাহ পরে

রাত দশটায় হিসাব মেলাতে বসে রাফি দেখল, ১০৪২ নম্বর order-এর টাকা ফেরত হয়ে গেছে। রাফি নিজে করেনি।

কর্মী খারাপ মানুষ ছিল না। কৌতূহল ছিল, বা একটা ভুল। Order-এর পাতার URL ছিল `/orders/1042`। Refund-এর URL তাহলে কী হতে পারে, সেটা আন্দাজ করতে বেশি সময় লাগে না। একটা সরাসরি request গেল। Server দেখল, request-টা চেনা user-এর। তারপর refund করে দিল।

আরিফের মাথায় একটা কথা ঘুরতে লাগল: button তো লুকানো ছিল।

কিন্তু button কোনো নিয়ম নয়। Button একটা সুবিধা। যে পাতা browser-এ যায়, সেটা user-এর হাতে। User সেটা দেখতে পারে, বদলাতে পারে, বা পাতা না খুলেই সরাসরি request পাঠাতে পারে। পুরনো bookmark, কারও পাঠানো link, browser-এর history, অনুমান — রাস্তা অনেক। নিয়ম শুধু UI-তে থাকলে রাস্তা খুঁজে পাওয়াই যথেষ্ট।

Server-এর নিয়ম থাকবে server-এ।

## আসলে দুটো আলাদা প্রশ্ন

আরিফের handler একটা প্রশ্নের উত্তর দিয়েছিল, কিন্তু আরেকটা প্রশ্ন জিজ্ঞেসই করেনি।

> **Authentication** হলো “তুমি কে?” সেটা verify করার প্রক্রিয়া।
>
> **Authorization** হলো “তুমি কি এই কাজটা করতে পারো?” সেটা ঠিক করার প্রক্রিয়া।

রাফির দোকানের কথাই ভাবো। কর্মীকে রাফি চেনে। রোজ দেখে, নাম জানে। কিন্তু চেনা মানেই ক্যাশবাক্সের চাবি দেওয়া নয়। কর্মীর পরিচয় একটা জিনিস। কর্মীকে কোন কাজের ভার দেওয়া হয়েছে, সেটা আরেকটা সিদ্ধান্ত।

মানুষের সম্পর্কে আমরা এই দুটো প্রায়ই গুলিয়ে ফেলি। যাকে চিনি, তাকেই বিশ্বাস করি। Software-এ এই অভ্যাস বিপজ্জনক। Server কাউকে চিনলে সে শুধু জানে, মানুষটা কে। মানুষটা কী কী করতে পারবে, সেটা আলাদা করে ঠিক করতে হয়।

| | Authentication | Authorization |
| --- | --- | --- |
| প্রশ্ন | তুমি কে? | তুমি কি এটা করতে পারো? |
| কখন | আগে | authentication-এর পরে |
| ফল | একটা পরিচয় (user), বা “চিনতে পারিনি” | অনুমতি আছে, বা নেই |
| ব্যর্থ হলে | request-এ বৈধ credential নেই | চেনা গেছে, কিন্তু কাজটা চলবে না |
| HTTP status | সাধারণত `401` | সাধারণত `403` |
| রাফির দোকানে | কর্মী দোকানে ঢুকল | কর্মী ক্যাশবাক্স খুলতে চাইল |

ক্রমটা গুরুত্বপূর্ণ। Authorization তার ইনপুট পায় authentication-এর কাছ থেকে। “এই user কি refund করতে পারে?” প্রশ্নটার কোনো মানে নেই, যদি আগে জানা না থাকে user-টা কে। Authentication ভুল হলে authorization ঠিকঠাক কাজ করেও ভুল মানুষকে অনুমতি দিতে পারে।

```text
Request
   |
   v
+--------------------+
| Authentication     |   “তুমি কে?”
+--------------------+
   |            \
   | চেনা গেল     \ চেনা গেল না
   v               v
+--------------------+   401
| Authorization      |
|                    |   “তুমি কি এটা করতে পারো?”
+--------------------+
   |            \
   | অনুমতি আছে   \ অনুমতি নেই
   v               v
কাজটা হলো         403
```

### 401 আর 403

HTTP-র নিয়ম RFC 9110-এ। সেখানে `401` মানে request-এ target resource-এর জন্য বৈধ authentication credential নেই। `403` মানে server request বুঝেছে, কিন্তু সেটা করতে রাজি নয়।

নামটা বিভ্রান্তিকর। `401`-এর নাম “Unauthorized”, কিন্তু ঘটনাটা মূলত authentication-এর। আসল authorization-এর “না” আসে `403`-এ। নাম নয়, প্রশ্নটা মনে রাখো: চিনতে পারিনি, নাকি চিনেছি কিন্তু অনুমতি নেই।

## ঠিক সমাধান: Server প্রতিটা request-এ জিজ্ঞেস করবে

আরিফ এবার নিয়মটা server-এ বসাল। দোকানে এখন দুই ধরনের মানুষ, মালিক আর কর্মী। এই অধ্যায়ের জন্য একটা খুব সরল ছক:

```go
type User struct {
    ID      int64
    Email   string
    IsOwner bool // এই অধ্যায়ের সরল ছক। পরে organization আর role আসবে (অধ্যায় ১৫)
}

type Action string

const (
    ViewOrders  Action = "orders:view"
    RefundOrder Action = "orders:refund"
)

func can(u *User, a Action) bool {
    if u == nil {
        return false // অচেনা কেউ কিছুই পারে না
    }
    switch a {
    case ViewOrders:
        return true // login করা যে কেউ
    case RefundOrder:
        return u.IsOwner
    default:
        return false // নিয়মে লেখা নেই মানে অনুমতি নেই
    }
}
```

Handler এখন দুটো আলাদা প্রশ্ন করে।

```go
func refundHandler(w http.ResponseWriter, r *http.Request) {
    user := currentUser(r)

    // প্রশ্ন ১: তুমি কে?
    if user == nil {
        http.Error(w, "আগে login করো", http.StatusUnauthorized) // 401
        return
    }

    // প্রশ্ন ২: তুমি কি এটা করতে পারো?
    if !can(user, RefundOrder) {
        http.Error(w, "এই কাজের অনুমতি তোমার নেই", http.StatusForbidden) // 403
        return
    }

    refund(r.PathValue("id"))
    w.WriteHeader(http.StatusNoContent)
}
```

এবার কর্মী সরাসরি `/orders/1042/refund`-এ request পাঠালে `403` পায়। Button থাকুক বা না থাকুক, তাতে কিছু যায় আসে না। Button এখন শুধু সুবিধা: যে কাজটা করার অনুমতি নেই, সেটা কেন দেখাব। নিয়মটা ওই handler-এ।

এখানে কয়েকটা নীতি কাজ করছে। OWASP-এর Authorization Cheat Sheet আর Developer Guide-এ এগুলোর কথা আছে।

- **Deny by default।** নিয়মে স্পষ্ট করে অনুমতি না থাকলে উত্তর “না”। `can`-এর `default` শাখা সেই কাজ করছে।
- **Least privilege।** মানুষকে ততটুকুই ক্ষমতা, যতটুকু তার কাজের জন্য লাগে। কর্মী order সামলায়, তাই refund তার হাতে নেই।
- **প্রতিটা request-এ check।** আগের request-এ অনুমতি ছিল বলে পরেরটায় ধরে নেওয়া যায় না।
- **একটা জায়গায় নিয়ম।** নিয়ম প্রতিটা handler-এ ছড়িয়ে থাকলে একটা handler-এ ভুলে গেলে সেটাই ফাঁক।

Least privilege-এর আরেকটা উপকার আছে, যেটা প্রথমে চোখে পড়ে না। কোনোদিন কর্মীর অ্যাকাউন্ট অন্য কারও হাতে গেলে সে কর্মীর ক্ষমতাটুকুই পাবে। রাফির ক্ষমতা পাবে না। Authorization শুধু ভুল আটকায় না। অ্যাকাউন্ট চুরি হলে ক্ষতির আকারও সীমিত রাখে।

OWASP-এর API Security Top 10 (2023 সংস্করণ)-এ এই ধরনের ভুলের নাম আছে। কোনো কাজ বা endpoint-এ অনুমতি ঠিকভাবে না দেখলে সেটা Broken Function Level Authorization। আর কোনো নির্দিষ্ট resource, যেমন একটা order, কার সেটা না দেখলে Broken Object Level Authorization, সংক্ষেপে BOLA। দ্বিতীয়টা আমরা অধ্যায় ১৫-তে বিস্তারিত দেখব, যখন KenaKata-য় একাধিক organization আসবে।

## Trade-off

এই সমাধানের দাম আছে।

- **প্রতিটা নতুন endpoint-এ মনে রাখতে হয়।** Check ভুলে গেলে সেটা কোনো error দেয় না। Endpoint নিঃশব্দে খোলা থেকে যায়। এই ঝুঁকি কমাতে নিয়মটা এমনভাবে বসাতে হয়, যাতে ভুলে গেলে ফল “না” হয়, “হ্যাঁ” নয়। এখানেই deny by default-এর দাম আর দরকার, দুটোই।
- **নিয়ম বাড়ে।** আজ রাফি আর কর্মী, দুটো ধরন। কাল আরও মানুষ আর আরও কাজ এলে `IsOwner` একটা bool দিয়ে আর চলবে না। এটা ইচ্ছে করেই সরল রাখা হয়েছে। পরে role আসবে।
- **কর্মীর কাজে বাধা।** কর্মী একদিন বলবে, “রাফি নেই, একটা refund এখনই দরকার।” Security আর কাজের গতির এই টানাপোড়েন যাবে না। কাকে কতটা ক্ষমতা, সেটা একটা ব্যবসায়িক সিদ্ধান্ত।
- **`403` নাকি `404`।** `403` বললে কর্মী জানতে পারে, ওই পাতাটা আছে। কিছু সিস্টেম লুকাতে `404` দেয়। দুটোরই দাম আছে: লুকালে ভুল বুঝে কেউ সময় নষ্ট করে, না লুকালে একটা তথ্য বেরিয়ে যায়। এই অধ্যায়ে `403` রাখলাম, কারণ বিষয়টা পরিষ্কার দেখাতে চাই।

## এই সমাধান কী সমাধান করে না

- **ভুল মানুষকে চেনা।** Authorization ধরে নেয় authentication ঠিক আছে। কেউ রাফির কর্মীর পরিচয় নিয়ে ঢুকে পড়লে `can` তাকে কর্মীই ভাববে। পরিচয় ঠিকভাবে চেনা আমাদের এখনো বাকি: password আর session নিয়ে অধ্যায় ৩ আর ৪।
- **Resource কার।** কর্মী `ViewOrders` করতে পারে, সেটা জানা গেল। কিন্তু কোন order সে দেখতে পারবে? এখন একটাই দোকান, তাই প্রশ্নটা চাপা আছে। একাধিক organization এলে এটা আসল সমস্যা হবে। অধ্যায় ১৫।
- **অনুমতি বদলে গেলে।** রাফি যদি কাল কর্মীকে ছাড়িয়ে দেয়, তাহলে ওই কর্মীর চলতে থাকা login-এর কী হবে? Logout আর revocation নিয়ে অধ্যায় ১৪।
- **সবচেয়ে সংবেদনশীল কাজ।** Refund-এর জন্য শুধু “মালিক” হওয়া যথেষ্ট কি না, সেই প্রশ্ন এখন তুলছি না। সেটা অনেক পরে, অধ্যায় ২২-এ।

## What did we learn?

- Authentication হলো “তুমি কে?”। Authorization হলো “তুমি কি এই কাজটা করতে পারো?”। দুটো আলাদা প্রশ্ন।
- Authorization-এর ইনপুট authentication-এর ফল। তাই ক্রম গুরুত্বপূর্ণ।
- “Login করা আছে” মানে শুধু এটুকু যে server user-কে চেনে। সব কাজের অনুমতি তার মানে নয়।
- UI-তে button লুকানো সুবিধা, নিয়ম নয়। নিয়ম থাকবে server-এ, প্রতিটা request-এ।
- `401` আর `403` আলাদা ঘটনা। চিনতে পারিনি, বনাম চিনেছি কিন্তু অনুমতি নেই।
- নিয়মে লেখা না থাকলে উত্তর হবে “না” (deny by default)। আর মানুষকে ততটুকুই ক্ষমতা, যতটুকু দরকার (least privilege)।

## One uncomfortable question

তোমার সবচেয়ে সংবেদনশীল endpoint-টার কথা ভাবো। এই মুহূর্তে সেটা কি শুধু “user login করা কি না” দেখছে?

কেউ তোমার UI একবারও না খুলে, সরাসরি request পাঠালে কী হবে? তুমি কি সেটা জানো, নাকি আন্দাজ করছ?

## আরও পড়ো

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)
- [RFC 9110 — HTTP Semantics, Client Error 4xx](https://www.rfc-editor.org/rfc/rfc9110.html#name-client-error-4xx)

[Season 1](../README.md) · [সিরিজের সূচি](../../../README.md)

## লেখার অবস্থা

- [x] Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published