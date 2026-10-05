# Browser security

Browser behavior and the cookie specification both move. A chapter that depends on a default, such as a default `SameSite` value, must be checked against current browser documentation and the current cookie draft or RFC at the time of writing. Do not copy a default into the product docs as a permanent fact.

## Cookies

- [MDN: HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
- [MDN: Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)
- [RFC 6265 — HTTP State Management Mechanism](https://www.rfc-editor.org/rfc/rfc6265)

RFC 6265 is the published cookies RFC. The active revision is still a draft and should be labeled as a draft if a chapter cites it:

- [draft-ietf-httpbis-rfc6265bis — Cookies: HTTP State Management Mechanism](https://httpwg.org/http-extensions/draft-ietf-httpbis-rfc6265bis.html)

## SameSite

`SameSite` is a `Set-Cookie` attribute. Read the current `Set-Cookie` documentation and the cookie draft above. Do not treat one browser’s default as the attribute’s meaning.

## Secure

`Secure` is a `Set-Cookie` attribute. Its meaning is in the cookie documents above, not in a separate product rule.

## HttpOnly

`HttpOnly` is a `Set-Cookie` attribute. A chapter that discusses it has to say what script access it removes and what attacks it does not stop. The misconception list already reserves “HttpOnly prevents XSS.”

## Origin

- [MDN: Origin header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin)
- [MDN: Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
- [HTML Living Standard: Origin](https://html.spec.whatwg.org/multipage/browsers.html#origin)

## CORS where relevant

CORS is a browser rule for reading responses across origins. It is not an authorization system for KenaKata’s API.

- [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Fetch Living Standard](https://fetch.spec.whatwg.org/)
