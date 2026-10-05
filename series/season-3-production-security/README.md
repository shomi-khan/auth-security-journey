# Season 3 — Production Security

Chapters 22–25. None of these chapters are written yet.

## Purpose

Take the controls from the first two seasons into a system that has sensitive operations, more than one service, and a final architecture. The capstone is the place those pieces are read as one design, with the trade-offs still visible.

The final picture is in [`../../product/architecture.md`](../../product/architecture.md). Earlier chapters must not treat that picture as the system they are already running.

## Chapters

22. [MFA ও Step-up Authentication](22-mfa-step-up/README.md) — MFA and step-up for sensitive operations.
23. [Machine-to-Machine Authentication](23-machine-to-machine/README.md) — Authentication between services.
24. [Security Context ও Architecture](24-context-and-architecture/README.md) — Security context and the production architecture.
25. [Capstone](25-capstone/README.md) — KenaKata’s production security as one system.

## After this season

The reader should be able to:

- Say which operations need step-up, and why a normal session is a different control
- Separate a person’s credential from a service’s credential
- Read the final architecture as a set of boundaries: client, gateway, service, authorization, identity provider, and connected application
- Trace one KenaKata action from the first shop’s password to the production control that now applies to it

[Series index](../../README.md)
