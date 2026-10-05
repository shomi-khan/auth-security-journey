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
User login করা থাকা অবস্থায় অন্য সাইট থেকে তার নামে request পাঠানো। বিস্তারিত অধ্যায় ৬।

## XSS

সহজ ভাষায়:
অন্যের পাঠানো script যখন আমাদের page-এ চলে। বিস্তারিত অধ্যায় ৭।

## Token

সহজ ভাষায়:
Client যেটা দেখিয়ে বলে, এই request কোন পরিচয়ের। Cookie session-এর বাইরে এই কথাটা অধ্যায় ৮-এ আসে।

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
