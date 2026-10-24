# Glossary

শুরুর ছোট তালিকা। অধ্যায় লেখার সঙ্গে এটা বাড়বে।

এখানে পুরো ব্যাখ্যা নেই। প্রথমবার নামটা দেখলে যেটুকু লাগে, সেটুকু। আক্রমণ, সীমা, আর trade-off অধ্যায়ে থাকবে।

## Authentication

সহজ ভাষায়:
User আসলে কে, সেটা verify করার প্রক্রিয়া।

## Authorization

সহজ ভাষায়:
Authenticated user কোন কাজ বা resource access করতে পারবে, সেটা নির্ধারণ করার প্রক্রিয়া।

## Session

সহজ ভাষায়:
Login-এর পর server যে অবস্থা মনে রাখে, যাতে পরের request-এ আবার password না লাগে।

## Cookie

সহজ ভাষায়:
Browser যে ছোট ডেটা জমিয়ে রাখে এবং পরের request-এর সঙ্গে পাঠাতে পারে।

## CSRF

সহজ ভাষায়:
Browser-এর স্বয়ংক্রিয় credential (যেমন session cookie) ব্যবহার করে user-এর ইচ্ছার বাইরে তার নামে কোনো site-এ request করানো। আক্রমণকারীকে cookie বা response পড়তে হবে এমন নয়। বিস্তারিত অধ্যায় ৬।

## Synchronizer Token

Server-side session-এর সঙ্গে যুক্ত একটি গোপন, অনুমান-অযোগ্য মান। Page সেটি form field বা request header-এ দেয়, আর server state-changing request গ্রহণের আগে session-এর মানের সঙ্গে মিলিয়ে দেখে। বিস্তারিত অধ্যায় ৬।

## Fetch Metadata

Browser-এর request header-এর একটি গোষ্ঠী, যেমন `Sec-Fetch-Site`, যা request কোন context থেকে এসেছে তা জানায়। এটি signal, authentication নয়; non-browser client header জাল করতে পারে। বিস্তারিত অধ্যায় ৬।

## XSS

সহজ ভাষায়:
অন্যের পাঠানো script যখন আমাদের page-এ চলে। বিস্তারিত অধ্যায় ৭।

## Token

সহজ ভাষায়:
কোনো client request-এ বহন করা মান, যা server-কে পরিচয় বা নির্দিষ্ট access grant যাচাই করতে সাহায্য করে। Token নিজে সবসময় user identity বোঝায় না, আর token থাকা মানেই সব resource বা কাজের অনুমতি নয়। Cookie session-এর বাইরে access-token model অধ্যায় ৮-এ আসে।

## Access Token

সহজ ভাষায়:
Resource ডাকতে যে token দেখানো হয়। মেয়াদ আর বাতিল করার কথা পরের অধ্যায়ে।

## Refresh Token

সহজ ভাষায়:
নতুন access token আনতে যে token ব্যবহার হয়। কেন আলাদা, সেটা অধ্যায় ১২।

## JWT

সহজ ভাষায়:
JSON Web Token। Token লেখার একটা format। কী নিশ্চিত করে আর কী করে না, সেটা অধ্যায় ৯।

## OAuth 2.0

সহজ ভাষায়:
কোনো application-কে password না দিয়ে নির্দিষ্ট access দেওয়ার প্রোটোকল। পুরো ব্যাখ্যা অধ্যায় ১৬ থেকে।

## PKCE

সহজ ভাষায়:
Authorization code-এর সঙ্গে যোগ হওয়া একটা ধাপ। কাকে দরকার, সেটা অধ্যায় ১৯।

## OIDC

সহজ ভাষায়:
OpenID Connect। OAuth 2.0-এর উপর identity-র স্তর। অধ্যায় ২০।

## ID Token

সহজ ভাষায়:
OIDC-তে identity নিয়ে যে token আসে। Access token-এর সঙ্গে পার্থক্য অধ্যায় ২১।

## MFA

সহজ ভাষায়:
Multi-factor authentication। একের বেশি উপায়ে পরিচয় দেখা। অধ্যায় ২২।

## Step-up Authentication

সহজ ভাষায়:
আগে থেকে login থাকা সত্ত্বেও কোনো সংবেদনশীল কাজে আরেক ধাপ চাওয়া। অধ্যায় ২২।

## Session Hijacking

চালু session-এর বৈধ পরিচয়পত্র, যেমন session ID-যুক্ত cookie, চুরি বা দখল করে ব্যবহার করা। বিস্তারিত অধ্যায় ৫।

## Session Fixation

আক্রমণকারী আগে থেকে জানা বা নিজের বেছে নেওয়া session ID-তে victim-কে login করাতে পারে, আর login-এর পর সেই ID-ই বৈধ থাকলে আক্রমণকারী session ব্যবহার করতে পারে। Login-এর সময় ID rotate করা গুরুত্বপূর্ণ। বিস্তারিত অধ্যায় ৫।

## Idle Timeout

শেষ activity-র পর নির্দিষ্ট সময় request না এলে session বাতিল হওয়ার সীমা। আক্রমণকারী নিজেই request পাঠিয়ে এই ঘড়ি চালু রাখতে পারে।

## Absolute Timeout

Session login হওয়ার পর সর্বোচ্চ কতক্ষণ বৈধ থাকবে, তার সীমা। Activity হলেও এই ঘড়ি নতুন করে শুরু হয়।
