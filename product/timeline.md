# Timeline

This is the product chronology. It exists so a chapter does not use a feature before KenaKata has it.

The chapter order is the teaching order. Where a chapter number and this file disagree about what already exists, this file wins.

Two teaching details do not change the chronology:

- Refresh tokens enter the product with the mobile app. Chapters 12–14 explain them after the SPA chapter. They are already in the product by the time the SPA exists. Chapter 10 may mention that the mobile client has them, and leave rotation and revocation for later chapters.
- Chapters 16–20 teach OAuth, PKCE, and OIDC while Google login and third-party connections are still requirements, not shipped features. Google login ships in chapter 21. A connected external application ships after that, at the end of Season 2.

## Simple E-commerce

KenaKata is one server-rendered shop: products, customer records, and orders. Rafi’s clothing business is that shop. People use the application directly.

Not yet: accounts as a designed system, sessions, mobile, SPA, organizations, Google, connected apps, MFA, or a split backend.

## Email + Password

Staff sign in with email and password. The password is a secret the server must store carefully. There is still one shop, not a tenant model.

## Session + Cookie

The server keeps a session and the browser carries the session identifier in a cookie. This is the sign-in mechanism for the server-rendered web application.

## Security Incidents

The same cookie session is attacked. Hijacking, fixation, CSRF, and XSS are failures of this stage, not new products. The architecture is still one web application.

## Mobile App

A mobile client for the owner and staff is added. The browser cookie is no longer the only way a client proves a sign-in.

## Access Token + Refresh Token

The mobile client uses an access token and a refresh token. JWT may be the access-token format under discussion. The server-rendered site can still use its cookie session.

## SPA

A single-page browser application is added beside the original web app. Token storage in the browser becomes a product problem. Refresh tokens already exist because mobile shipped first.

## Organizations + Authorization

The single shop becomes an organization. Another business can join. Membership and the Owner, Admin, Member, and Viewer roles exist. Products, customers, and orders belong to an organization, so “a signed-in user” is no longer enough to decide access.

Rafi’s clothing business is now one organization.

## Google Login

A user can sign in with Google. `ExternalIdentity` exists. KenaKata is the relying party. Google is the identity provider. Connected third-party applications are still not a shipped feature.

## Third-party Integrations

An organization can connect an external application. `OAuthClient` exists. For these apps, KenaKata is the authorization server. That is the reverse of Google login.

## MFA + Sensitive Operations

Sensitive operations require more than the existing sign-in. MFA and step-up authentication are part of the product. The factor type (TOTP, passkey, or something else) is not fixed in this timeline. A chapter that chooses one should say it is the choice for that scene.

The sensitive operations are listed in [`product.md`](product.md).

## Microservices

The backend is no longer one process. User, order, billing, and notification services sit behind an API gateway. Services authenticate to each other. The product catalog does not gain its own service in this series.

## Production Security Architecture

The pieces above are one system: clients, gateway, services, authorization, Google, OAuth clients, and step-up on sensitive operations. That picture is [`architecture.md`](architecture.md). It is not the architecture of the early chapters.

## Chapter map

| Chapters | Product stage the chapter may assume |
| --- | --- |
| 1 | Simple e-commerce. Authentication is the problem, not a finished design |
| 2 | The same early shop. Authorization is distinguished from authentication, not implemented as organizations |
| 3 | Email and password exist |
| 4 | Session and cookie exist |
| 5–7 | The cookie session is under attack. Tokens are not the current design |
| 8–9 | The mobile app exists. Access tokens are introduced. JWT is the format under discussion |
| 10 | The SPA exists. Mobile refresh tokens may already exist |
| 11–14 | Expiration, refresh, rotation, logout, and revocation for those tokens |
| 15 | Organizations, membership, roles, and resource ownership exist |
| 16–20 | Delegation is the problem being designed. Google login is not shipped. External apps are not a finished integration |
| 21 | Google login ships |
| End of season 2 | Third-party integrations ship after chapter 21 |
| 22 | MFA and step-up ship |
| 23 | Microservices and machine-to-machine authentication ship |
| 24–25 | The final architecture may be discussed as the production picture |
