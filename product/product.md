# KenaKata

KenaKata (কেনাকাটা, “shopping”) is a fictional SaaS commerce platform for small businesses. It is the only product in this series. Chapters may show it at an earlier stage, but they may not describe a different product.

This file is a writing reference, not a chapter.

## Product overview

A small business uses KenaKata to run its shop: products, customers, and orders, then staff, billing, and connected applications. People who sign in are the business owner and staff. A shopper is a customer record, not a user account.

The story starts with one shop and ends as a multi-tenant SaaS platform with a web application, a single-page application, and a mobile application.

## Initial product

At the start there is one online shop and one server-rendered web application. The shop can list products, keep customer records, and take orders.

There is no organization model, no roles, no mobile app, no SPA, no Google login, no connected third-party application, no MFA, and no microservice split. Those arrive only at the stages in [`timeline.md`](timeline.md).

Rafi’s clothing business is that first shop. It is not named as a separate brand.

## Target users

- A small-business owner who needs the shop to run without building software. Rafi is the concrete case.
- Staff who help that owner with products and orders.
- Later, more than one business on the same platform, each isolated as an organization.

Shoppers do not get KenaKata accounts in this series.

## Business model

KenaKata starts as the application for a single shop. It becomes a subscription product sold to organizations. Billing is part of the product and is treated as a sensitive operation.

This document does not fix prices, plan names, or currency. A chapter that needs a number should treat it as a scene detail, not as a new canonical price list.

## Core features

These are the product’s features across the whole series. The timeline says when each one is available.

- Manage products
- Manage customers
- Manage orders
- Create teams
- Manage organization members
- Perform billing-related operations
- Connect external applications
- Use web and mobile applications

## Product evolution

The shop gains a sign-in, then a cookie session, then tokens because a mobile client appears. A browser SPA follows. The single shop then becomes one organization among many, with membership and roles. Google login comes next, then external applications connected through OAuth. Sensitive operations gain MFA and step-up. The backend later splits into services behind an API gateway.

Details and the “not yet” line for each stage are in [`timeline.md`](timeline.md). The end state is in [`architecture.md`](architecture.md).

## Web application

The first interface is a server-rendered web application. Sign-in is a browser session held by a cookie. This is the application in the early chapters.

The server-rendered application can remain after the SPA exists. The series does not require it to be removed.

## SPA

A single-page application is added later as another browser client for the same business. It is a separate interface from the first server-rendered app. Where its tokens live is a browser problem, which is why it has its own chapter.

## Mobile application

The mobile application is for the owner and staff. It is added when a browser cookie is no longer the only way someone uses KenaKata. It is the reason the product grows access tokens and refresh tokens.

## Organizations

An organization is one small business on the platform. Its products, customers, orders, members, teams, billing, and connected applications belong to that organization.

Before this stage, the story has one shop. After it, Rafi’s clothing business is one organization, and another business can join without seeing Rafi’s data. A person can be a member of more than one organization.

## Teams

A team is a group of members inside one organization, used to organize work such as who handles orders. A team does not have its own role system. What a person may do comes from their membership role.

## Third-party integrations

An organization can connect an external application so that application can act on the organization’s data. That external application is an OAuth client, and KenaKata is the authorization server.

This is a different relationship from Google login. For Google login, Google is the identity provider and KenaKata is the client.

## Sensitive operations

These actions are dangerous enough that a normal signed-in session is not treated as sufficient once the product reaches the MFA stage:

- Change billing or a payment method
- Invite or remove a member, or change a member’s role
- Transfer ownership
- Connect or disconnect an external application
- Delete an organization, or export its data

Earlier chapters may show these actions as ordinary requests. They become step-up actions only when the timeline says MFA exists.

## Admin functionality

Administration is an organization role, not a separate KenaKata staff product. The Owner holds the organization. An Admin runs day-to-day management, including members and connected apps, and does not transfer ownership. The role sketch is in [`domain-model.md`](domain-model.md).

There is no platform-superadmin character. Do not add one without a decision-log entry.

## Final product state

At the end of the series, KenaKata is a multi-tenant SaaS platform:

- Web, SPA, and mobile clients for owners and staff
- Organizations, memberships, roles, and teams
- Products, customers, and orders isolated per organization
- Google as an external identity provider
- External applications connected with OAuth
- MFA and step-up on sensitive operations
- An API gateway in front of separate user, order, billing, and notification services
- An authorization layer in front of the protected action

That end state is drawn in [`architecture.md`](architecture.md). Chapters 1–24 may use only the part of it that the timeline has already introduced.
