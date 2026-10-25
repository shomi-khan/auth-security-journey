---
chapter: 15
season: 1
title: "IDOR/BOLA এবং Authorization Design"
status: planned
---

# IDOR/BOLA এবং Authorization Design

> **অবস্থা:** পরিকল্পনা (Brief প্রস্তুত, পূর্ণাঙ্গ অধ্যায় লেখা হয়নি)

## অধ্যায়ের কাজ

Authentication ঠিক থাকলেও একজন user অন্যের order দেখতে বা বদলাতে পারে, যদি server নির্দিষ্ট resource-এর ওপর তার অনুমতি যাচাই না করে। এই অধ্যায় object-level authorization-এর মানসিক মডেল তৈরি করবে এবং KenaKata-র এক দোকান থেকে multi-organization SaaS-এ যাওয়ার সময় authorization design কীভাবে বদলায় তা দেখাবে।

মূল শিক্ষা: **user কে, তা জানা আর user এই নির্দিষ্ট resource-এ এই কাজটি করতে পারবে কি না, তা জানা এক প্রশ্ন নয়।**

## গল্পের শুরু

রাফি KenaKata-তে সঠিকভাবে login করেছে। সে নিজের order দেখার সময় request-এ থাকা order ID বদলে অন্য customer-এর order দেখতে পায়। Login সফল ছিল, token-ও বৈধ ছিল। সমস্যা পরিচয় যাচাইয়ে নয়, server ওই order-এর ওপর রাফির অনুমতি যাচাই করেনি।

এরপর KenaKata এক দোকান থেকে বহু organization-কে সেবা দেওয়া SaaS-এ পরিণত হয়। এখন একই user এক organization-এর member হতে পারে, অন্যটির নয়। শুধু resource ID বা user role দেখে সিদ্ধান্ত নিলে tenant boundary ভেঙে যেতে পারে।

## মূল ধারণা

- **IDOR (Insecure Direct Object Reference):** client-প্রদত্ত object reference ব্যবহার করার সময় resource-level permission যাচাই না হওয়ায় অননুমোদিত access বা modification।
- **BOLA (Broken Object Level Authorization):** API security-তে একই ধরনের object-level authorization failure বোঝানোর পরিভাষা। শব্দ দুটি সব context-এ নিখুঁত synonym নয়, তবে এই অধ্যায়ে তাদের সাধারণ security lesson হলো object-এর ওপর অনুমতি যাচাই করা।
- **Function-level authorization:** user কোনো function বা endpoint ব্যবহার করতে পারে কি না, যেমন administrative action।
- **Object-level authorization:** user নির্দিষ্ট order, invoice, document বা অন্য object-এর ওপর চাওয়া action করতে পারে কি না।
- **Subject, action, resource, context:** কে কাজ করছে, কী কাজ করতে চায়, কোন resource-এ এবং কোন প্রাসঙ্গিক শর্তে, এই চারটি প্রশ্নে authorization decision ব্যাখ্যা করা।
- **Tenant boundary:** user-এর organization membership, resource-এর organization এবং সেই scope-এ role/permission মিলিয়ে দেখা।

## আলোচ্য বিষয়

- URL, query, header বা request body-তে ID থাকা কেন permission নয়
- Sequential ID বদলানোই একমাত্র attack নয়; UUID বা অনুমান করা কঠিন identifier থাকলেও authorization check আবশ্যক
- Authentication, function-level permission ও object-level permission-এর পার্থক্য
- Subject-action-resource-context দিয়ে authorization decision বোঝানো
- Organization membership, role এবং tenant isolation
- Server-side enforcement: read, list, update, delete, export এবং nested resource
- List endpoint ও single-object endpoint-এ একই access boundary বজায় রাখা
- Query scoping ও centralized authorization logic-এর trade-off; query filter-কে business permission-এর সম্পূর্ণ বিকল্প হিসেবে না দেখানো
- Deny-by-default, least privilege এবং প্রতিটি প্রাসঙ্গিক request-এ authorization
- 401, 403 ও 404-এর সাধারণ অর্থ; 404 দিয়ে resource existence গোপন করার policy থাকলে সেটি consistent হওয়া
- Negative authorization test: user A দিয়ে user B-এর resource, এবং organization A দিয়ে organization B-এর resource চেষ্টা করা
- অধ্যায় ২-এর authorization ধারণা এবং [product timeline](../../../product/timeline.md)-এর organization model-এর সঙ্গে সংযোগ

## অধ্যায় শেষে পাঠক পারবে

- Authentication failure আর authorization failure আলাদা করতে
- একটি request-এ function-level ও object-level check-এর প্রয়োজনীয়তা শনাক্ত করতে
- Authorization decision-কে subject, action, resource ও context দিয়ে ব্যাখ্যা করতে
- Multi-tenant data isolation-এর threat model তৈরি করতে
- Cross-user, cross-organization ও nested-resource negative test case প্রস্তাব করতে
- অনুমান করা কঠিন ID কেন authorization-এর বিকল্প নয় তা ব্যাখ্যা করতে

## Diagram ও security invariant

কমপক্ষে দুটি Mermaid diagram থাকবে:

1. **একটি resource পড়া বা বদলানো:** authentication, function permission এবং object-level authorization আলাদা ধাপে দেখানো; permission না থাকলে operation বন্ধ হবে।
2. **Multi-tenant access decision:** user-এর active organization membership, resource-এর tenant এবং requested action মিলিয়ে সিদ্ধান্ত নেওয়া; tenant mismatch হলে deny path দেখানো।

প্রয়োজনে তৃতীয় diagram-এ list endpoint ও nested resource-এর authorization boundary দেখানো যেতে পারে।

**মূল invariant:**
- Request-এ resource ID থাকলেই access অনুমোদিত হয় না।
- Resource-এ কাজ করার আগে server-কে authenticated subject-এর সেই resource ও action-এর permission যাচাই করতে হবে।
- Multi-tenant system-এ resource-এর tenant boundary ও user-এর membership যাচাই না করে শুধু role বা object ID দেখে অনুমতি দেওয়া যাবে না।
- অনুমতি না থাকলে operation চলবে না; response code ও resource-existence গোপন করার আচরণ policy অনুযায়ী consistent হতে হবে।

Runnable code থাকবে না। Mermaid হবে প্রধান teaching tool; pseudocode কেবল diagram দিয়ে authorization decision পরিষ্কারভাবে বোঝানো না গেলে ব্যবহার করা যাবে।

## এই অধ্যায়ে থাকবে না

- OAuth delegation flow; অধ্যায় ১৬–২০
- পূর্ণাঙ্গ RBAC/ABAC policy engine implementation
- নির্দিষ্ট database, framework বা programming language-এর tutorial
- Object property-level authorization-এর পূর্ণ আলোচনা, যেমন কোন field পড়া বা বদলানো যাবে; এটি আলাদা সমস্যা হিসেবে সংক্ষেপে চিহ্নিত করা যেতে পারে
- API abuse, rate limiting বা sensitive business-flow abuse-এর পূর্ণ আলোচনা

## সূত্র যাচাইয়ের নির্দেশনা

- [OWASP API Security Top 10: API1:2023 Broken Object Level Authorization](https://api-security.owasp.org/editions/2023/en/0xa1-broken-object-level-authorization/): object ID ব্যবহার করা প্রতিটি প্রাসঙ্গিক endpoint-এ permission যাচাইয়ের গুরুত্ব বোঝাতে।
- [OWASP IDOR Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html): IDOR-এর কারণ, server-side authorization এবং identifier সম্পর্কে প্রচলিত ভুল ধারণা যাচাই করতে।
- [OWASP API Security Top 10: API5:2023 Broken Function Level Authorization](https://api-security.owasp.org/editions/2023/en/0xa5-broken-function-level-authorization/): function-level ও object-level failure-এর পার্থক্য পরিষ্কার করতে।
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html): deny-by-default, least privilege ও request-level enforcement-এর guidance যাচাই করতে।

OWASP-এর API guidance-কে KenaKata-র জন্য নির্দিষ্ট framework implementation হিসেবে উপস্থাপন করা যাবে না। পূর্ণাঙ্গ draft লেখার সময় সংশ্লিষ্ট উৎস আবার যাচাই করতে হবে।

## ধারাবাহিকতা

আগে: [অধ্যায় ১৪: Logout, Revocation এবং Session Recovery](../14-logout-revocation/README.md)

পরে: [অধ্যায় ১৬: OAuth 2.0 কেন?](../../season-2-oauth-oidc/16-oauth/README.md)

[Product timeline](../../../product/timeline.md) অনুযায়ী KenaKata এখন organization, membership এবং Owner, Admin, Member, Viewer role-সহ multi-organization SaaS। এই অধ্যায় সেই product model-এ authorization failure বোঝাবে; OAuth বা external identity flow নয়।

## লেখার অবস্থা

- [ ] পূর্ণাঙ্গ Draft
- [ ] Technical Review
- [ ] Security Review
- [ ] Published
