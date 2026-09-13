# REQUIREMENTS.md — AppSmith Requirements

> Requirements discovered over the life of the project. Add to this file as requirements are confirmed, refined, or changed. Never delete a superseded requirement — move it to the "Superseded / Changed Requirements" section with a note on what replaced it and why.

**Last updated:** 2026-09-13

## Confirmed Requirements

### Functional
- FR-1: User describes a web app in free-text chat; system generates a working React+Vite web app from it. (DECISIONS.md D-1, D-3)
- FR-2: System renders a live preview of the generated app during the build/iterate loop. (D-1)
- FR-3: User can iterate on the app conversationally (multi-round chat), steering direction as in Lovable's CAP-001 pattern. (D-1, D-5)
- FR-4: User can download the generated app as a real, working buildable/deployable artifact (zip/dist) to self-host and test locally. (D-2)
- FR-5: Projects persist server-side per user account, independent of sign-in/sign-out; user resumes an earlier project on next sign-in without re-uploading anything. (D-6)
- FR-6: Each project is a real git repo; each meaningful build round is a commit; user can revert the project to any earlier checkpoint ("state as of \<date\>"). (D-6)
- FR-7: AppSmith itself (the platform) supports user accounts with both email/password and OAuth sign-in. (D-7)
- FR-8: Account auth (signup, signin, OAuth, password reset, sessions) is implemented via a managed auth provider, not hand-rolled. Specific provider not yet chosen — see OPEN-QUESTIONS.md. (D-8)

### Non-Functional
- NFR-1: Build/execution of generated code happens in a server-side sandbox (Docker/VM per project), not purely client-side, so real build artifacts can be produced. (D-4)
- NFR-2: Project is developed across many separate multi-session Claude Code sessions — architecture and process must not depend on continuous conversation context.

## Tentative Requirements
_Requirements under discussion, not yet confirmed. Move to Confirmed once settled, or to Superseded if dropped/changed._

- One-click hosting of generated apps directly by the platform (real end-goal per user, but deferred — see DECISIONS.md D-2 and IDEAS.md).

## Constraints
_Hard limits the project must operate within (technical, business, personal-time, etc.)._

- First slice excludes: Android/mobile output, a managed backend *offered to generated apps* (i.e. AppSmith does not give end-user-generated apps their own DB/auth/storage, unlike Lovable's Supabase integration), billing/credits, third-party integrations (Shopify-equivalent), security scanning, SEO tooling, collaboration/sharing, template gallery. (DECISIONS.md D-1)
- Note: this is distinct from FR-7/FR-8 — AppSmith's *own* platform-level user accounts (for signing into AppSmith itself) ARE in scope; it's only a managed backend feature *inside generated apps* that's excluded.

## Superseded / Changed Requirements
_Requirements that changed over time. Keep the original wording, the date/session it changed, and why. Do not delete history._

_None yet._

---
*Cross-reference: requirements often originate from a DECISIONS.md entry or a BRAINSTORMING.md conclusion — link back where relevant.*
