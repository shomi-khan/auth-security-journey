# Architecture

This document describes the **final-state architecture**. It is what KenaKata looks like at the end of the series. The service split arrives with the microservices stage in chapter 23. Chapters 24 and 25 are where this picture is the current system. Earlier chapters must not treat it as the system they are already running, except when a chapter is explicitly looking ahead.

The diagram is conceptual. It is not an implementation, a deployment, or a request path with headers and status codes.

## Final picture

```text
Web / SPA
Mobile App
    |
    v
API Gateway
    |
    +---- User Service
    +---- Order Service
    +---- Billing Service
    +---- Notification Service
    |
    v
Authorization Layer
```

Clients talk to the API gateway. The gateway routes to the service that owns the data. The authorization layer is how a protected action is allowed or refused. In this conceptual picture it sits on the path of the protected action. A chapter does not need to turn it into a specific library or a sidecar.

The product catalog is not a separate service. Catalog data stays with the commerce side of the model.

| Piece | Role in the final system |
| --- | --- |
| Web / SPA | Browser clients for owners and staff. The original server-rendered app and the later SPA may both exist |
| Mobile App | Client for owners and staff, using tokens |
| API Gateway | The front door for client traffic after the split |
| User Service | Accounts, sign-in, sessions, and external identities |
| Order Service | Orders for an organization |
| Billing Service | Billing-related operations |
| Notification Service | Messages the product sends, such as mail |
| Authorization Layer | The decision that this identity may perform this action on this resource |

## Google as an external identity provider

Google is an identity provider. KenaKata is the relying party. A successful Google login creates or matches a `User` and stores an `ExternalIdentity`. Google is not the place KenaKata stores orders, and a Google account is not an organization role.

## OAuth-based external applications

An organization can connect an external application. That application is an `OAuthClient`. It calls KenaKata for the organization’s resources, and KenaKata is the authorization server for that call. This is the opposite direction from Google login.

## Service-to-service communication

After the microservice split, services call each other. Those calls authenticate the calling service. They do not reuse a person’s browser session or a person’s refresh token. Machine-to-machine authentication is the subject of chapter 23.

## MFA for sensitive operations

Sensitive operations, listed in [`product.md`](product.md), require step-up even when the caller already has a session or an access token. MFA is part of that final control. It is not shown as a sixth business service in the diagram.

## What this picture leaves out

There is no message bus, service mesh, cloud account, or platform-admin console in the series architecture. Do not add one in a chapter unless the decision log takes it on.
