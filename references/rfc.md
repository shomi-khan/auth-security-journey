# IETF / RFC

অফিসিয়াল লেখা RFC Editor-এ। নাম ইংরেজিতেই রাখা। লেখা এখানে তোলা হয়নি।

দাবি লেখার আগে RFC-এর বর্তমান কপি খুলে দেখো। Errata থাকলে সেটাও। পুরনো RFC আর Best Current Practice না মিললে BCP-কে সামনে রাখো। অধ্যায়ে বলো পুরনো ডকুমেন্টটা কী, আর বর্তমান নির্দেশনা কোনটা।

## RFC 6749 — The OAuth 2.0 Authorization Framework

- কী বিষয়: OAuth 2.0-এর মূল প্রোটোকল।
- কোন অধ্যায়: ১৬–২১।
- কেন পড়া useful: Authorization code, token, আর client কীভাবে access পায়, তার মূল লেখা এখানে। এটা একা বর্তমান security নির্দেশনা নয়। সেই জায়গায় RFC 9700 দেখো।
- [RFC 6749](https://www.rfc-editor.org/rfc/rfc6749)

## RFC 7636 — Proof Key for Code Exchange (PKCE)

- কী বিষয়: Authorization code-এর সঙ্গে PKCE।
- কোন অধ্যায়: ১৯। Code flow আর code চুরির অধ্যায় ১৭–১৮-এর সঙ্গেও মিলিয়ে পড়া যায়।
- কেন পড়া useful: PKCE কী যোগ করে, সেটা এই RFC না পড়ে অধ্যায়ে লিখবে না।
- [RFC 7636](https://www.rfc-editor.org/rfc/rfc7636)

## RFC 7519 — JSON Web Token (JWT)

- কী বিষয়: JWT ফরম্যাট।
- কোন অধ্যায়: ৯। অধ্যায় ২১-এ ID token যখন JWT হিসেবে আসে, তখনও।
- কেন পড়া useful: Claim, স্বাক্ষর, আর ফরম্যাটের মূল সংজ্ঞা এখানে। “JWT মানে কী নিরাপত্তা” এই RFC একা বলে দেয় না।
- [RFC 7519](https://www.rfc-editor.org/rfc/rfc7519)

কাছাকাছি দুটো, যেগুলো অধ্যায় ৯ আর ২০-এ লাগতে পারে:

- [RFC 7515 — JSON Web Signature (JWS)](https://www.rfc-editor.org/rfc/rfc7515)
- [RFC 7517 — JSON Web Key (JWK)](https://www.rfc-editor.org/rfc/rfc7517)

## RFC 8725 — JSON Web Token Best Current Practices

- কী বিষয়: JWT ব্যবহারের বর্তমান ভালো চর্চা।
- কোন অধ্যায়: ৯, ১১, আর ১৪।
- কেন পড়া useful: ফরম্যাট জানার পরে যে ভুলগুলো হয়, সেগুলো মেলাতে এই BCP দরকার। কী এড়াতে বলে, সেটা পড়ে অধ্যায়ে তুলবে। এখানে তোলা হয়নি।
- [RFC 8725](https://www.rfc-editor.org/rfc/rfc8725)

## RFC 9700 — Best Current Practice for OAuth 2.0 Security

- কী বিষয়: OAuth 2.0-এর বর্তমান security চর্চা।
- কোন অধ্যায়: ১৬–২১। Refresh token-এর অধ্যায় ১২–১৪-এর সঙ্গেও মিলিয়ে দেখো।
- কেন পড়া useful: এটা RFC 6749, RFC 6750, আর RFC 6819-এর threat model আর security পরামর্শকে সামনে এগিয়েছে। কিছু পুরনো চালু পদ্ধতি এখানে বদলেছে। কী বদলেছে, অধ্যায়ে লেখার আগে নিজে পড়তে হবে।
- [RFC 9700](https://www.rfc-editor.org/rfc/rfc9700)

আগের threat catalog, যেটা 9700-এর বদলে ব্যবহার করবে না, বরং সঙ্গে পড়বে:

- [RFC 6819 — OAuth 2.0 Threat Model and Security Considerations](https://www.rfc-editor.org/rfc/rfc6819)

## RFC 9449 — OAuth 2.0 Demonstrating Proof of Possession (DPoP)

- কী বিষয়: Access token যেন শুধু চুরি করে অন্য জায়গা থেকে ব্যবহার করা না যায়, সেই ধরনের একটা প্রোটোকল।
- কোন অধ্যায়: Token আবার ব্যবহারের কথা অধ্যায় ১১-এ, Season 2-এ, আর service-এর পরিচয় অধ্যায় ২৩-এ। DPoP তখনই আনবে, যখন অধ্যায় সত্যি এই প্রশ্ন তোলে।
- কেন পড়া useful: Bearer token-এর বাইরে কী বিকল্প আছে, সেটা এই RFC-এ। কীভাবে কাজ করে, এখানে লেখা নেই।
- [RFC 9449](https://www.rfc-editor.org/rfc/rfc9449)

## কাছাকাছি আরও কয়েকটা

এগুলো আলাদা প্রোটোকল পরিবার নয়। মেয়াদ, বাতিল করা, আর access token কেমন হতে পারে — এই প্রশ্নে খুলে দেখো।

- [RFC 6750 — Bearer Token Usage](https://www.rfc-editor.org/rfc/rfc6750)। অধ্যায় ৮, ৯, ১৬।
- [RFC 7009 — Token Revocation](https://www.rfc-editor.org/rfc/rfc7009)। অধ্যায় ১৪।
- [RFC 7662 — Token Introspection](https://www.rfc-editor.org/rfc/rfc7662)। অধ্যায় ১৪ আর ২৩।
- [RFC 9068 — JWT Profile for OAuth 2.0 Access Tokens](https://www.rfc-editor.org/rfc/rfc9068)। অধ্যায় ৯ আর ১৬। প্রতিটা JWT-কে access token ধরে নিয়ো না। এই প্রোফাইলটা কখন লাগে, পড়ে দেখো।
