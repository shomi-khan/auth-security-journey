---
chapter: 14
season: 1
title: "Logout, Revocation এবং Session Recovery"
status: planned
---

# Logout, Revocation এবং Session Recovery

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ

Logout button চাপলে আসলে কী শেষ হয়? Client-এর local state মুছে ফেলা, server-side session বা refresh token revoke করা, আর self-contained access token-এর কার্যকর মেয়াদ শেষ হওয়া এক ঘটনা নয়। এই অধ্যায় এসব পার্থক্য এবং incident-এর পর recovery policy ব্যাখ্যা করবে।

মূল প্রশ্ন হবে: UI login screen-এ ফিরে গেলেই কি চুরি হওয়া credential আর ব্যবহার করা যাবে না? উত্তর architecture ও revocation policy-র ওপর নির্ভর করে।

## গল্পের শুরু

রাফি KenaKata-র mobile app থেকে logout করে। UI login screen-এ ফিরে যায়, কিন্তু আরিফ জানে local state পরিষ্কার হওয়া মানেই server-side credential revoke হয়েছে, এমন নয়। পুরোনো self-contained access token এখনও expiry পর্যন্ত গ্রহণযোগ্য হতে পারে। পরে এক কর্মীর ফোন হারিয়ে যায়, আর নীরাকে ঠিক করতে হয় কোন session বা credential বাতিল করতে হবে এবং অন্য device-গুলোতে কী হবে।

## আলোচ্য বিষয়

- **Client-side logout:** local credential ও authenticated UI state পরিষ্কার করা; offline হলে server-side revocation নিশ্চিত হয়েছে বলে দাবি না করা
- **Server-side revocation:** session, refresh token বা token family নিষ্ক্রিয় করা
- **Access token-এর ধরন ও revocation:** server-side lookup/introspection-নির্ভর token বনাম self-contained JWT-এর trade-off
- **JWT-এর expiry:** signature যাচাই করা যায় বলেই token revoke হওয়ার পরও সেটি গ্রহণ করতে হবে, এমন নয়; কিন্তু revocation state না দেখলে resource API expiry-এর আগে token-টিকে গ্রহণ করতে পারে
- **Short lifetime, denylist এবং server-side state:** দ্রুত revocation-এর সুবিধা, request-time lookup, cache staleness, latency ও operational complexity
- **Logout from one device বনাম logout all devices:** একটি session/device-এর credential বন্ধ করা আর সব session revoke করা এক সিদ্ধান্ত নয়
- **Password reset, account compromise এবং employee offboarding:** কোন credential/session বাতিল হবে, user notification ও recovery কী হবে
- **Refresh token বাতিল বনাম access token বাতিল:** refresh বন্ধ করলেই আগে ইস্যু করা প্রতিটি access token সঙ্গে সঙ্গে অকার্যকর হয় না
- **Audit log:** ঘটনা ও ফলাফল রেকর্ড করা, কিন্তু raw access/refresh token log না করা
- **User-facing message:** “এই device থেকে logout হয়েছে” এবং “সব session revoke হয়েছে” এক দাবি নয়

## অধ্যায় শেষে পাঠক পারবে

- Local logout, token/session revocation ও expiry আলাদা করে ব্যাখ্যা করতে
- JWT access token তাৎক্ষণিক revoke করা কেন architecture-dependent তা বলতে
- হারানো device বা compromised account-এর জন্য scope-সহ recovery policy প্রস্তাব করতে
- এক device logout এবং সব device logout-এর ফল আলাদা করে নির্ধারণ করতে
- Security, availability, latency ও operational complexity-এর trade-off তুলনা করতে
- Revocation request ব্যর্থ হলে UI ও server-side state সম্পর্কে সঠিক দাবি করতে

## Diagram ও security invariant

কমপক্ষে দুটি Mermaid diagram থাকবে:

1. **স্বাভাবিক logout:** client local state পরিষ্কার করে, server-side revocation চেষ্টা করে, এবং success/failure অনুযায়ী UI কী বলতে পারে।
2. **হারানো device বা account compromise:** একটি session বনাম সব session revoke করা, refresh credential বন্ধ করা এবং access token কখন থেকে আর গ্রহণযোগ্য হবে তা দেখানো।

Diagram-এ client, application/token-issuing component এবং protected resource API-র ভূমিকা আলাদা থাকবে। Failure path-এ network unavailable বা revocation ব্যর্থ হলে local logout এবং server-side revocation-এর পার্থক্য দেখাতে হবে।

**মূল invariant:**
- Local logout সফল হওয়া মানেই server-side credential revoke হয়েছে, এমন নয়।
- Revocation-এর পর access token গ্রহণ বন্ধ হবে কি না, তা নির্ভর করে resource API revocation state পরীক্ষা করে কি না এবং cache/policy কীভাবে কাজ করে তার ওপর।
- User-কে “সব device থেকে logout হয়েছে” বলা যাবে কেবল সংশ্লিষ্ট সব session/credential-এর revocation policy সফলভাবে প্রয়োগ হয়েছে বলে system নিশ্চিত হতে পারলে।
- Audit log-এ raw bearer/access/refresh token রাখা যাবে না।

Runnable code থাকবে না। Mermaid diagram হবে প্রধান teaching tool; pseudocode কেবল diagram দিয়ে মূল logic বোঝানো না গেলে বিবেচনা করা যাবে।

## এই অধ্যায়ে থাকবে না

- OAuth token revocation endpoint-এর পূর্ণ protocol specification
- MFA বা step-up authentication; অধ্যায় ২২
- নির্দিষ্ট vendor বা framework-এর implementation
- JWT মানেই সর্বদা self-contained, অথবা সব JWT-ই revocation ছাড়া চলবে, এমন সাধারণীকরণ
- Refresh token rotation/reuse detection-এর পূর্ণ আলোচনা; অধ্যায় ১৩

## সূত্র যাচাইয়ের নির্দেশনা

- [RFC 7009 — OAuth 2.0 Token Revocation](https://www.rfc-editor.org/rfc/rfc7009.html): OAuth revocation-এর ধারণা এবং refresh token revoke করলেই access token তাৎক্ষণিক invalid হবে এমন নিশ্চয়তা না থাকার বিষয় যাচাই করতে। OAuth endpoint-এর পূর্ণ specification এই অধ্যায়ের scope নয়।
- [RFC 7662 — OAuth 2.0 Token Introspection](https://www.rfc-editor.org/rfc/rfc7662.html): protected resource কীভাবে token-এর active state যাচাই করতে পারে, এবং cache করলে freshness বনাম load-এর trade-off বোঝাতে।
- [RFC 7519, §4.1.4 — Expiration Time](https://www.rfc-editor.org/rfc/rfc7519.html#section-4.1.4): JWT-এর exp claim-এর অর্থ বোঝাতে। exp নিজে server-side revocation mechanism নয়।
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html): session invalidation ও logout-এর security consideration যাচাই করতে।

এই standards-এর OAuth-specific normative requirements-কে KenaKata-র first-party mobile flow-এর সরাসরি protocol specification হিসেবে উপস্থাপন করা যাবে না। Draft লেখার সময় প্রাসঙ্গিক section আবার খুলে যাচাই করতে হবে।

## ধারাবাহিকতা

আগে: [অধ্যায় ১৩: Refresh Token Rotation এবং Reuse Detection](../13-refresh-token-rotation/README.md)

পরে: [অধ্যায় ১৫: IDOR/BOLA এবং Authorization Design](../15-idor-bola-authorization/README.md)

[Product timeline](../../../product/timeline.md) অনুযায়ী mobile app-এ access token ও refresh token আগে থেকেই আছে। এই অধ্যায় first-party mobile flow-তে logout, revocation এবং recovery-র ধারণা ব্যাখ্যা করবে, OAuth token revocation endpoint-এর tutorial নয়।

## লেখার অবস্থা

- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
