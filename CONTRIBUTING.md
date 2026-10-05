# Contributing

Chapters are added one at a time. The manuscript for a chapter already has a directory. Edit that chapter’s `README.md`. Do not open a new chapter folder, and do not skip ahead by writing several chapters in one change.

**Do not break continuity with the KenaKata product universe.**

Before drafting, read the current [`product/`](product/README.md) files. If a scene needs a feature, character, or architecture that those files do not allow yet, change the scene. Do not quietly change KenaKata.

## Workflow

```text
1. Select the next chapter
2. Review product/ files
3. Review previous chapter
4. Draft chapter
5. Verify technical/security claims
6. Add diagrams/examples if needed
7. Update chapter status
8. Update season README
9. Update root README
```

1. **Select the next chapter.** Write the next unpublished chapter in order, unless the root index says otherwise.
2. **Review `product/`.** Use [`product.md`](product/product.md), [`characters.md`](product/characters.md), [`domain-model.md`](product/domain-model.md), [`timeline.md`](product/timeline.md), and [`architecture.md`](product/architecture.md). The timeline decides what already exists.
3. **Review the previous chapter.** The new chapter continues its problem. It does not reset the product.
4. **Draft the chapter** in that chapter’s `README.md`. Bengali is the chapter language. Replace the placeholder. Keep the metadata block, and keep it accurate.
5. **Verify technical and security claims** against [`references/`](references/README.md). Prefer the primary specification or an OWASP cheat sheet over a blog. Re-read the source in the same change as the claim. Record a corrected misconception in [`notes/misconceptions.md`](notes/misconceptions.md) when the chapter depends on it.
6. **Add diagrams or examples only if the chapter needs them.** Prefer a short ASCII diagram in the chapter. Put a reusable figure under [`diagrams/`](diagrams/authentication/). A Go example under [`examples/go/`](examples/go/authentication/) must stay small, teach one idea, and remain readable on its own. It must not grow into a backend for KenaKata.
7. **Update the chapter status.** Tick a checklist item only when that step is done. Set the metadata `status` to `published` only when the chapter is published.
8. **Update the season README** if the chapter’s status or one-line description changed.
9. **Update the root README.** Leave the chapter index at `[ ]` until the chapter is published, then mark that entry `[x]`.

## Continuity

- The recurring people are Arif, Nira, and Rafi. Add another recurring person only with an entry in [`notes/decision-log.md`](notes/decision-log.md).
- Shoppers stored as customers do not sign in.
- Roles are Owner, Admin, Member, and Viewer, and they live on organization membership.
- Google login, connected external applications, MFA, and microservices appear only at the timeline stage that introduces them.
- [`product/architecture.md`](product/architecture.md) is the final system. Early chapters are still the single web application described in the timeline.

## Chapter metadata

Keep this block at the top of every chapter and use the same fields:

```yaml
---
chapter: 1
season: 1
title: "কেন Authentication দরকার?"
status: draft
---
```

`status` is `draft` until the chapter is published.

## Writing rules

### Language

Primary language: Bengali.

Technical terms can remain in English when that is clearer.

Repository guides outside `series/` stay in English so the product universe is easy to check while drafting.

### Style

- narrative-driven
- problem-first
- technically rigorous
- beginner-accessible
- production-aware
- occasional subtle humor
- no meme-heavy writing

Follow the series arc:

```text
Problem
  ↓
Naive Solution
  ↓
Failure
  ↓
Threat
  ↓
Better Solution
  ↓
Trade-off
```

### Security

Never use unexplained absolute claims such as “JWT is secure.”

Always explain:

- threat model
- security property
- limitation
- trade-off

### Major concepts

Every major security technology should explain:

- what it is
- why it exists
- what problem it solves
- what it does not solve
- where it lives
- how it can be attacked
- damage if compromised
- lifetime
- revocation
- alternatives
- production trade-offs

## What does not belong here

Do not add a site generator, application, package manifest, Go module, or CI pipeline for the series itself. This repository is Markdown, plus a small example when a chapter needs one.
