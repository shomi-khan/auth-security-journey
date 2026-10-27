# Editorial Review: Chapters 1–16

**Date:** 2026-10-27  
**Scope:** Cross-chapter consistency, reader-facing clarity, chapter navigation, and conceptual security wording.  
**Result:** Edits are in branch `docs/review-chapters-01-16`; this is not a production security audit.

## Main corrections

- Aligned cross-references with the current 26-chapter plan: MFA/step-up is Chapter 23, machine-to-machine authentication is Chapter 24, logout/revocation is Chapter 15, and organization/object-level authorization is Chapter 16.
- Clarified the session lifecycle: an anonymous pre-session may exist before login for a return destination or login-CSRF protection; an authenticated session must only be created after successful authentication, with a new unpredictable ID and invalidation of the pre-session.
- Updated cookie-standard references in Chapter 4 to RFC 10025, which supersedes RFC 6265, and removed the now-obsolete draft-status citation.
- Corrected the NIST AAL2 timeout summary in Chapter 5 to label inactivity (1 hour) and overall (24 hours) separately.
- Added consistent YAML front matter to Chapters 9–12.
- Removed the internal Chapter 12 planning brief from the reader-facing article and preserved it in `notes/chapter-brief-12.md`.
- Aligned root README, Season 1 README, and product timeline with the repository's actual state: Chapters 1–16 have full drafts; Chapters 17–26 remain in planning.
- Clarified that password-reset's complete flow is not a separate chapter; Chapter 15 covers the session/credential invalidation and recovery consequences.

## Editorial rules reinforced

1. Authentication, session continuity, CSRF, XSS, token validation, and authorization are separate decisions and should not be presented as interchangeable controls.
2. Browser behavior examples and diagrams are conceptual unless a tested browser matrix or runnable test is explicitly documented.
3. OAuth-specific normative requirements must not be presented as if KenaKata's first-party mobile flow were an OAuth implementation.
4. The reader-facing article should not contain internal chapter-planning briefs.

## Verification limits

- Repository content and cross-references were inspected; several wording and metadata edits were committed to the review branch.
- No application code, browser compatibility matrix, or production configuration was tested.
- Chapter review checkboxes remain governed by each chapter's own review record. This editorial pass does not constitute production certification.
