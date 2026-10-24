# Chapter 7: Technical ও Security Review

**তারিখ:** 2026-10-25  
**Branch:** `improvements/chapters-engagement`  
**Reviewed file:** `series/season-1-browser-session-token/07-xss-and-httponly/README.md`  
**Review type:** Source-based conceptual review of the chapter, its claims, diagrams, and cited guidance.

## সারাংশ

Chapter 7-এর প্রথম Draft-এর Technical ও Security Review করা হয়েছে। Review-এ একটি গুরুত্বপূর্ণ taxonomy issue পাওয়া গিয়েছিল: Stored, Reflected এবং DOM-based XSS-কে এমনভাবে উপস্থাপন করা হয়েছিল, যাতে পাঠক এগুলোকে পরস্পর-বিচ্ছিন্ন তিনটি category ভাবতে পারত। এটি সংশোধন করা হয়েছে। আরও দুটি clarification যোগ হয়েছে: safe DOM text sink-এর উদাহরণ এবং server-side authorization-এর অব্যাহত ভূমিকা।

এই review নথি chapter-এর source-based conceptual accuracy নিয়ে। এটি implementation audit, browser compatibility test বা production security certification নয়।

## Technical Review

### ১. XSS classification

**আগের gap:** Stored/reflected input delivery এবং server-side/DOM-based unsafe processing-এর পার্থক্য যথেষ্ট স্পষ্ট ছিল না।

**সংশোধন:** Chapter এখন আলাদা করে ব্যাখ্যা করে:
- Stored/reflected হলো attacker-controlled input victim-এর কাছে পৌঁছানোর delivery path।
- Server-side/DOM-based হলো unsafe processing কোথায় ঘটে।
- Stored/reflected input browser-side code-এ unsafeভাবে handle হলে DOM-based XSS-ও হতে পারে।

**Source:** OWASP DOM-based XSS Prevention Cheat Sheet  
https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html

### ২. Safe DOM sink

Chapter-এ plain text দেখানোর জন্য `textContent`-এর উদাহরণ যোগ করা হয়েছে এবং unsafe HTML/code-interpreting sink-এর বিপরীতে সেটি ব্যাখ্যা করা হয়েছে। এটিকে সর্বক্ষেত্রের encoding strategy হিসেবে দেখানো হয়নি: attribute, URL, CSS, script এবং rich HTML-এর জন্য আলাদা context-aware handling দরকার।

**Sources:**
- https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html
- https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html

### ৩. Context-specific output encoding ও sanitization

Chapter সঠিকভাবে আলাদা করেছে:
- Plain text render করার জন্য destination context অনুযায়ী output encoding / framework escaping।
- User-authored rich HTML-এর জন্য maintained HTML sanitizer।
- Sanitization-এর পরে unsafe modification করলে সুরক্ষা নষ্ট হতে পারে।
- Input validation একা output encoding বা sanitization-এর বিকল্প নয়।

### ৪. CSP-এর ভূমিকা ও সীমা

Chapter CSP-কে defense-in-depth হিসেবে উপস্থাপন করে, root-cause fix-এর বিকল্প হিসেবে নয়। Nonce/hash policy এবং Report-Only mode-এর সীমা ব্যাখ্যা করা হয়েছে। Report-Only violation report করতে পারে, কিন্তু নিজে execution block করে না।

**Source:** OWASP Content Security Policy Cheat Sheet  
https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html

### ৫. HttpOnly ও browser request

Chapter সঠিকভাবে আলাদা করেছে:
- HttpOnly cookie value-কে JavaScript cookie API থেকে আড়াল করে।
- একই origin-এর vulnerable page-এ চলা script user-এর browser দিয়ে request শুরু করতে পারে।
- Cookie value না পড়তে পারা মানেই authenticated request করতে না পারা নয়।

Chapter-এ এই দাবিকে সীমাবদ্ধ করা হয়েছে: request সফল হবে কি না, তা cookie scope/policy, browser behavior, CSP/network restrictions, এবং server-side controls-এর ওপরও নির্ভর করে।

### ৬. Server-side authorization boundary

**সংশোধন:** Diagram ও ব্যাখ্যায় যোগ করা হয়েছে যে server-side authorization ও business-rule checks এখনও প্রযোজ্য। XSS মানেই attacker সব operation করতে পারবে, এমন নয়। তবে script user-এর বৈধ ক্ষমতা অপব্যবহারের চেষ্টা করতে পারে।

### ৭. Diagrams ও learning flow

- চারটি Mermaid diagram আছে।
- XSS delivery/processing taxonomy-র diagram সংশোধন করা হয়েছে।
- HttpOnly flow-তে cookie-read failure এবং server authorization outcome-এর পার্থক্য দেখানো হয়েছে।
- Chapter-এ threat model, limitation table, misconceptions, exercises এবং future manual-review checklist আছে।

**বাকি সীমা:** Mermaid source-এর structure পরিদর্শন করা হয়েছে, কিন্তু GitHub renderer বা কোনো browser-এ render করে দেখা হয়নি।

## Security Review

### Claims যাচাই

| Claim | Review outcome |
|---|---|
| HttpOnly JavaScript থেকে cookie value পড়া আটকায় | গ্রহণযোগ্য, cookie attribute-এর নির্দিষ্ট scope হিসেবে ব্যাখ্যা করা হয়েছে |
| HttpOnly একা XSS বন্ধ করে না | গ্রহণযোগ্য; OWASP guidance-এর সঙ্গে সামঞ্জস্যপূর্ণ |
| XSS script authenticated request শুরু করতে পারে | গ্রহণযোগ্য, তবে server authorization ও browser policy-র সীমা যোগ করা হয়েছে |
| অন্য origin-এর private data পড়া সাধারণত same-origin policy দ্বারা সীমিত | গ্রহণযোগ্য, নির্বিচার cross-origin access দাবি করা হয়নি |
| Output encoding destination context অনুযায়ী হওয়া দরকার | গ্রহণযোগ্য |
| Rich HTML-এর জন্য sanitization আলাদা control | গ্রহণযোগ্য |
| CSP defense-in-depth, একমাত্র প্রতিরক্ষা নয় | গ্রহণযোগ্য |
| Report-Only policy execution block করে না | গ্রহণযোগ্য |

### Threat-model scope

Chapter ধরে নিয়েছে attacker-controlled content page-এ পৌঁছাতে পারে এবং vulnerable rendering/browser-side processing-এর কারণে script চালু হতে পারে। Chapter ধরে নেয়নি যে attacker সব cookie, সব origin-এর data, operating system বা server database স্বয়ংক্রিয়ভাবে দখল করেছে।

### Review-এর সীমা

এই review-তে:
- Application source code পরীক্ষা করা হয়নি।
- Browser বা browser compatibility matrix চালানো হয়নি।
- Automated test, exploit reproduction বা dynamic security test চালানো হয়নি।
- CSP বা cookie configuration বাস্তবে deploy/test করা হয়নি।
- Reverse proxy, production deployment বা network egress policy পরীক্ষা করা হয়নি।

অতএব, এই review-কে source-based conceptual review হিসেবে গণ্য করতে হবে, implementation-level security audit বা production approval হিসেবে নয়।

## Status recommendation

Chapter-এ `Draft` checked আছে। `Technical Review` ও `Security Review` checkbox আপাতত unchecked রাখা হয়েছে, কারণ repository workflow-এ এই status-এর জন্য কোনো স্পষ্ট Chapter 7-specific completion rule পাওয়া যায়নি এবং browser/rendering/implementation validation করা হয়নি। এই নথি review-এর কাজ ও তার সীমা স্বচ্ছভাবে রেকর্ড করে; checkbox-কে বাস্তবের চেয়ে বেশি নিশ্চয়তা বোঝাতে ব্যবহার করা উচিত নয়।
