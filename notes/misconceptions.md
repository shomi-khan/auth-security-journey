# ভুল ধারণা

নিচে এমন কথা আছে, যেগুলো শুনতে সহজ, কিন্তু সিরিজে ভেঙে দেখতে হবে। ব্যাখ্যা এখনো নেই। অধ্যায় যখন কোনোটাকে ভাঙবে, তালিকায় ছোট করে যোগ হবে। আসল ব্যাখ্যা অধ্যায়েই থাকবে।

- JWT মানেই encryption
- OAuth মানেই login
- ID Token দিয়ে API call করা যায়
- HttpOnly থাকলে XSS সমস্যা নেই
- Short-lived token চুরি করা যায় না
- Logout করলে সব JWT সঙ্গে সঙ্গে invalid হয়ে যায়
- Token-এর মধ্যে role থাকলেই authorization complete
- Refresh Token মানেই নিরাপদ token

- Idle timeout থাকলেই hijack করা session বেশিক্ষণ চলতে পারে না
- Strict mode থাকলেই session fixation অসম্ভব
- Login-এর পর session record-এ শুধু user ID বসালেই চলে; session ID rotate করতে হয় না
- IP আর User-Agent মিলিয়ে রাখলেই session hijacking বন্ধ
- DBSC এলে cookie theft আর replay-এর ঝুঁকি শেষ
