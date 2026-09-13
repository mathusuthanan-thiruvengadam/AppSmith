# BRAINSTORMING.md — AppSmith Brainstorming Log

> Ideas, exploration, and conclusions from brainstorming sessions — including ones that haven't become a requirement or decision yet. This is the raw exploration trail; DECISIONS.md is the settled outcome, REQUIREMENTS.md is the settled scope.

**Last updated:** 2026-09-13

## How to log a brainstorming session

```
## <date> — <topic>
**Explored:** what was discussed
**Conclusions:** what was concluded (even if informal / not yet a formal decision)
**Open threads:** anything left dangling — cross-link to OPEN-QUESTIONS.md if unresolved
```

## Log

## 2026-09-13 — Lovable.dev architecture read + AppSmith first-slice scoping

**Explored:** Read full `documents/lovable-dev.md` reverse-engineering report. Discussed its Section 21 (Hypothetical Backend Requirements, HBR-1..10) as a possible architecture sketch for a Lovable-like platform: agent orchestration loop, isolated per-project compute, git-backed code storage, async job queue, credit ledger, integration broker, codebase index, security scan service, timeline/metadata store, workspace RBAC.

**Conclusions:** Full platform is too large for one build — decomposed into a first slice (web-only, core loop only) per DECISIONS.md D-1. User initially believed Lovable supported Android; verified via grep against the report that it's web-only — corrected before it became a wrong requirement.

**Open threads:** none — resolved into D-1 through D-4.

## 2026-09-13 — Persistence vs versioning distinction

**Explored:** User asked what happens to a project after sign-out if using the "flat files, no git" MVP option — worried the whole project would be lost and require re-uploading the downloaded zip to resume.

**Conclusions:** Corrected an unstated bad assumption baked into the original question — "no git" was only ever meant to mean "no checkpoint/revert history," not "no server-side persistence at all." Server-side persistence per account is basic expected behavior independent of git. Once separated, user confirmed wanting real git-backed, point-in-time revert (not just flat persisted current-state) — see DECISIONS.md D-6.

**Open threads:** none — resolved into D-6.

## 2026-09-13 — Auth: hand-rolled vs managed, and provider comparison

**Explored:** Compared hand-rolled auth vs three managed providers (Supabase Auth, Auth.js/NextAuth, Clerk) on both effort and money cost, then walked through the actual sign-up/sign-in/OAuth flow for each at a beginner level.

**Conclusions (cost):** At this project's scale (solo/early, low MAU), all three managed options and hand-rolled are effectively $0/month — Supabase Auth free to 50k MAU, Auth.js is a free open-source library, Clerk free to ~10k MAU, hand-rolled has near-zero vendor cost (only cheap/free transactional email for password reset). Money was not a differentiator; time/security-risk was → see DECISIONS.md D-8.

**Conclusions (provider flows, for future reference):**
- **Supabase Auth:** one SDK call for signup/signin/OAuth (`supabase.auth.signUp/signInWithPassword/signInWithOAuth`), Supabase stores users + sends verification/reset emails automatically. Bonus: also provides a free Postgres DB in the same project — could double as AppSmith's own platform DB (accounts, project metadata, git repo pointers).
- **Auth.js (NextAuth):** handles the OAuth redirect/token/cookie *flow* only — you still bring your own DB via an adapter (e.g. Prisma → Postgres) and write your own credential-check function. More wiring, more control, no vendor lock-in.
- **Clerk:** fastest to a working screen — pre-built `<SignUp />`/`<SignIn />` UI components, OAuth providers toggled on in a dashboard with zero code. Does not include a general-purpose DB — AppSmith's own data would still need a separate DB regardless.

**Open threads:** which of the three to actually use — see OPEN-QUESTIONS.md. Leaning signal: Supabase's bundled free DB is attractive since AppSmith needs a DB anyway, but not yet decided by user.

---
*Cross-reference: [DECISIONS.md](DECISIONS.md) for settled decisions, [IDEAS.md](IDEAS.md) for postponed ideas, [OPEN-QUESTIONS.md](OPEN-QUESTIONS.md) for unresolved threads.*
