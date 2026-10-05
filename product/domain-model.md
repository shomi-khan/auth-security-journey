# Domain model

This is a simplified conceptual model for the blog series, not a production database schema. Chapters may sketch a table when it helps a threat, but they should not grow a canonical schema here.

Names below are concepts. Fields are the minimum a chapter needs in order to stay consistent.

## Concepts

| Concept | What it is |
| --- | --- |
| User | A person who can sign in: an owner or a staff member |
| Customer | A shopper record kept by a business. Customers do not sign in |
| Organization | One small business on the platform, and the isolation boundary for its data |
| Membership | The link between a user and an organization, including that user’s role |
| Team | A named group of members inside one organization. Teams do not grant roles |
| Product | An item the organization sells |
| Order | A purchase recorded for that organization |
| Session | The server-side sign-in record introduced with browser cookies |
| OAuthClient | An external application authorized to act on an organization’s resources |
| ExternalIdentity | The link between a user and an account at an external identity provider |

`Session`, `OAuthClient`, and `ExternalIdentity` are part of the model so later chapters have stable names. They are not implemented at the start. [`timeline.md`](timeline.md) says when each one appears.

## Relationships

```text
User
  |
  +---- Membership ---- Organization
  |                         |
  |                         +---- Teams
  |                         +---- Products
  |                         +---- Customers
  |                         +---- Orders
  |                         +---- OAuthClients
  |
  +---- Sessions
  |
  +---- ExternalIdentities
```

A user can have a membership in more than one organization. A membership belongs to one user and one organization.

Products, customers, orders, teams, and connected clients belong to one organization. Before organizations exist, those business records belong to the single shop, which later becomes Rafi’s organization.

A team contains users who already have a membership in that organization.

A session belongs to one user. An external identity belongs to one user. An OAuth client is connected in the context of one organization.

An order may refer to products and to a customer. Those links are ordinary commerce data. They are not an authorization rule.

## Role model

Roles live on `Membership`. They do not live on the team, and they are not a second system inside the token.

```text
Owner
Admin
Member
Viewer
```

An organization has one Owner in this simplified model. Ownership can be transferred. That transfer is a sensitive operation.

| Role | May do |
| --- | --- |
| Owner | Everything an Admin may do, plus ownership transfer and the organization’s existence |
| Admin | Day-to-day management: members, roles except ownership, settings, connected applications, billing operations the product exposes to admins |
| Member | Create and update products, customers, and orders |
| Viewer | Read the organization’s data |

This table is a sketch so chapters do not contradict each other. A chapter may show one finer check, such as “only an Admin can invite a member,” without inventing a new role. Permission order stays Owner, then Admin, then Member, then Viewer.

A role string stored in a token is a hint about membership. It is not, by itself, the authorization decision. The resource still has to belong to the organization the membership is for.
