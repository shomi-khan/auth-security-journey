# Chapter 9: Technical ও Security Review

Date: 2026-10-25  
Branch: `improvements/chapters-engagement`  
Pull request: https://github.com/shomi-khan/auth-security-journey/pull/1

## Review scope

Chapter 9-এর source-based conceptual Technical Review ও Security Review। Chapter-এর core claims-কে RFC 7519, RFC 7515 এবং RFC 8725-এর সঙ্গে মিলিয়ে দেখা হয়েছে; chapter scope, diagrams, examples, validation guidance এবং threat-model boundaries যাচাই করা হয়েছে।

এই review কোনো application implementation audit, browser/mobile test, Mermaid renderer test, বা production security certification নয়।

## Findings and corrections

1. **JWT বনাম access token:** JWT-কে format হিসেবে এবং access token-কে usage/purpose হিসেবে আলাদা করা হয়েছে। সব JWT access token নয়, আর access token opaque হতে পারে।
2. **JWS structure:** তিন-part compact representation-কে common JWS Compact Serialization হিসেবে চিহ্নিত করা হয়েছে, সব JWT-এর universal structure হিসেবে নয়। JWE Compact Serialization-এ পাঁচটি অংশ থাকতে পারে।
3. **Encoding বনাম encryption:** Base64URL encoding payload-কে গোপন করে না, তা স্পষ্ট। Signed JWS-এর payload decode করা যায়; signature/MAC confidentiality দেয় না।
4. **Signing input diagram:** Mermaid diagram সংশোধন করে encoded header ও encoded payload-কে signing input হিসেবে দেখানো হয়েছে; signature/MAC-এর ভূমিকা আলাদা করা হয়েছে।
5. **Signature বনাম trust:** Valid signature একাই issuer trust, intended audience, expiry বা authorization নিশ্চিত করে না। Trusted key, server-defined algorithm policy, issuer/audience এবং প্রযোজ্য time claims-এর conceptual checks রাখা হয়েছে।
6. **Algorithm ও key selection:** Token-এর untrusted header-কে একা accepted algorithm বা trust source নির্ধারণ করতে দেওয়া যাবে না, এমন সতর্কতা যোগ করা হয়েছে।
7. **Authorization boundary:** Token-এর `role` claim resource/action permission-এর বিকল্প নয়। Decision Game-এ এই পার্থক্য অনুশীলন করানো হয়েছে।
8. **Scope control:** OAuth access-token profile, SPA token storage, expiry/replay timeline এবং refresh-token lifecycle নিজ নিজ পরের অধ্যায়ের জন্য রাখা হয়েছে।

## Sources checked

- RFC 7519, *JSON Web Token*: JWT claims and serialization definitions. https://www.rfc-editor.org/rfc/rfc7519.html
- RFC 7515, *JSON Web Signature (JWS)*: JWS signing input, compact serialization, signature/MAC integrity protection. https://www.rfc-editor.org/rfc/rfc7515.html
- RFC 8725, *JSON Web Token Best Current Practices*: security guidance for JWT deployments. https://www.rfc-editor.org/rfc/rfc8725.html
- Project source map: `references/rfc.md`.

## Review outcome and limitations

- Technical Review: complete, source-based conceptual review.
- Security Review: complete, source-based threat-model review.
- Mermaid diagrams were reviewed as source text only; they were not rendered in a Mermaid renderer.
- No application code, browser, or mobile tests were run.
- This record is not a production security audit or certification.
- Chapter status remains Draft; Published remains unchecked.
