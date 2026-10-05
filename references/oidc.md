# OpenID Connect

OpenID Foundation-এর স্পেসিফিকেশন errata দিয়ে বদলায়। নিচের লিংক এখন errata-সহ কপি খোলে। কোনো অনুচ্ছেদ তোলার আগে সেটা এখনো সত্যি কি না দেখে নাও। আগের Final লেখার চেয়ে errata-সহ কপি সামনে রাখো, দুটো না মিললে।

নতুন errata এসেছে কি না, সেটা এই তালিকায় দেখা যায়:

- [OpenID Connect specifications](https://openid.net/developers/specs/)

নাম ইংরেজিতেই থাকবে। স্পেসিফিকেশনের লেখা এখানে তোলা হয়নি।

## OpenID Connect Core

- কী বিষয়: OAuth 2.0-এর উপর identity-র স্তর। ID token আর user সম্পর্কে claim এখানে।
- কোন অধ্যায়: ২০ আর ২১।
- কেন পড়া useful: OIDC কী যোগ করে, তার মূল লেখা। এটা OAuth security BCP-এর বদলি নয়। দুটোই পড়তে হবে।
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)

## Discovery

- কী বিষয়: Relying party কীভাবে provider-এর endpoint আর key-এর জায়গা খুঁজে পায়।
- কোন অধ্যায়: ২০ আর ২১। Google login-এর সময় provider-এর তথ্য কোথা থেকে আসে, সেই প্রশ্নে।
- কেন পড়া useful: Endpoint হার্ডকোড করার আগে Discovery কী দেয়, সেটা এখান থেকে দেখো।
- [OpenID Connect Discovery 1.0](https://openid.net/specs/openid-connect-discovery-1_0.html)

## JWKS

`jwks_uri` OpenID Connect Discovery-তে আছে। ওই ঠিকানায় যে ডকুমেন্ট থাকে, সেটা একটা JWK Set।

- কী বিষয়: Provider-এর public key কীভাবে প্রকাশ করা হয়, যাতে ID token-এর স্বাক্ষর মেলানো যায়।
- কোন অধ্যায়: ২০ আর ২১।
- কেন পড়া useful: Google বা অন্য provider-এর token যাচাইয়ের কথা লেখার আগে key কোথা থেকে আসে, সেটা এই দুটো সোর্সে দেখো। নমুনা key এই repository-তে রাখবে না।
- [RFC 7517 — JSON Web Key (JWK)](https://www.rfc-editor.org/rfc/rfc7517)
- [OpenID Connect Discovery 1.0](https://openid.net/specs/openid-connect-discovery-1_0.html), `jwks_uri`-এর জন্য

## ID Token

- কী বিষয়: OIDC-তে যে token user-এর identity নিয়ে আসে। এটা আলাদা স্পেসিফিকেশন নয়। Core-এর অংশ।
- কোন অধ্যায়: ২০ আর ২১। অধ্যায় ২১ সরাসরি ID token আর access token আলাদা করে।
- কেন পড়া useful: ID token কী বহন করে আর কী কাজে লাগানো উচিত, সেটা Core না পড়ে লিখবে না। Access token-এর সঙ্গে গুলিয়ে ফেলার ভুল ধারণা [misconceptions](../notes/misconceptions.md)-এ আছে। সংশোধন অধ্যায়ে হবে।

## UserInfo

- কী বিষয়: Client provider-এর কাছে user-এর claim চাইতে পারে। এই endpoint-ও Core-এর অংশ।
- কোন অধ্যায়: ২০ আর ২১।
- কেন পড়া useful: ID token-এ যা আসে আর UserInfo থেকে যা আসে, দুটো এক কি না, সেটা Core পড়ে অধ্যায়ে লিখবে। এখানে পার্থক্য লিখে রাখা হয়নি।
