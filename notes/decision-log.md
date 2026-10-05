# Decision log

Major editorial and product decisions. Add a dated entry when a later chapter needs to change the universe, the language, or the structure.

## 2026-10-05 — Repository foundation

- Repository name: `auth-security-journey`
- Fictional product: `KenaKata`
- 25 chapters
- 3 seasons
- Problem-first narrative
- Bengali primary language for chapters
- Go examples where useful
- Security claims must use authoritative sources
- Chapter-by-chapter development

Related choices made so the foundation stays internally consistent:

- Guides outside `series/` are in English. Chapter titles are Bengali, and chapter bodies will be Bengali.
- Shoppers are customer records and do not sign in. Users are owners and staff.
- Roles are Owner, Admin, Member, and Viewer, and they live on membership. Teams group people and do not grant a second role.
- An organization has one Owner in the simplified model.
- There is no platform-superadmin character.
- The recurring cast is Arif, Nira, and Rafi.
- [`product/timeline.md`](../product/timeline.md) is the product chronology and overrides chapter order when they differ.
- [`product/architecture.md`](../product/architecture.md) is the final state only.
- The MFA factor type is not fixed.
- Prices and plan names are not fixed.
- The license is MIT, with `[COPYRIGHT HOLDER]` left as a placeholder because no copyright owner was specified.
