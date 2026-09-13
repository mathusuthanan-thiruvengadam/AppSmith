# DECISIONS.md — AppSmith Decision Log

> Every significant decision, permanently recorded. Never silently overwrite a decision. If a decision changes, keep the old entry as-is and add a new entry that references and supersedes it.

**Last updated:** 2026-09-13

## How to record a decision

Copy this template for each new entry, newest at the top:

```
### D-<number>: <short title>
- **Date/session:** YYYY-MM-DD
- **Decision:** what was decided
- **Context:** what prompted this decision
- **Reason/rationale:** why this choice
- **Alternatives considered:** list
- **Why alternatives were rejected:** reasoning per alternative
- **Consequences/trade-offs:** what this commits us to, what it costs
- **Status:** Active | Superseded by D-<number> | Reverted
```

## Decision Log

### D-8: Auth implementation = managed provider, not hand-rolled
- **Date/session:** 2026-09-13
- **Decision:** Use a managed auth provider/library rather than hand-rolling password hashing, reset flows, OAuth token exchange, and session management.
- **Context:** D-7 committed to supporting both email/password and OAuth, which doubles the hand-rolled auth surface. Needed to decide build-vs-buy for that surface.
- **Reason/rationale:** Managed provider = days of setup vs weeks hand-rolled, and auth is exactly the kind of security-sensitive code where mistakes leak user data. Cost check: at this project's scale (solo/early, low user count) all major managed options (Supabase Auth free to 50k MAU, Auth.js = free open-source library, Clerk free to ~10k MAU) are effectively $0/month — no meaningful money trade-off against hand-rolled (~$0 vendor cost either way, only a few dollars/month for transactional email if hand-rolled). Decision is purely a time/risk trade-off, and managed wins.
- **Alternatives considered:** Hand-rolled auth (bcrypt/argon2 hashing, custom reset-token + email flow, custom OAuth redirect/callback/token-exchange per provider, custom session/cookie handling).
- **Why alternatives were rejected:** No cost advantage, materially more dev time, more security risk, nothing gained since no unusual auth requirement exists here.
- **Consequences/trade-offs:** Depends on a third-party provider (uptime, pricing changes, API changes). Specific provider not yet chosen — see OPEN-QUESTIONS.md.
- **Status:** Active

### D-7: Auth methods = both email/password and OAuth
- **Date/session:** 2026-09-13
- **Decision:** Support both email/password sign-in and OAuth (e.g. Google/GitHub) for AppSmith accounts, for the first slice.
- **Context:** D-6 introduced the need for user accounts (server-side persistence is per-account). Needed to decide which sign-in method(s).
- **Reason/rationale:** User's explicit choice, made after being presented email/password-only, OAuth-only, and both as options.
- **Alternatives considered:** Email/password only (simplest, no OAuth app registration needed); OAuth only (no password storage/reset flow needed).
- **Why alternatives were rejected:** User chose maximum flexibility for sign-in over minimizing first-slice build surface; per ponytail guidance, an explicit user choice for the full version is honored, not re-argued.
- **Consequences/trade-offs:** More auth surface to build/secure than either option alone — mitigated by D-8 (managed provider).
- **Status:** Active

### D-6: Server-side persistence per account + git-backed checkpoints with revert
- **Date/session:** 2026-09-13
- **Decision:** Projects persist server-side per user account (files stored under account+project ID, independent of sign-in/sign-out) — user signs back in later and resumes, no re-upload needed. Each project is a real git repo; each meaningful agent build round = one commit; user can revert to any earlier checkpoint ("go back to the state on \<date\>"), matching Lovable's History panel + "Revert to this version" pattern.
- **Context:** Initial framing of "flat files, no git" for MVP incorrectly implied projects might not persist across sessions at all (re-upload downloaded zip to resume). User caught this — persistence across sessions is basic expected behavior regardless of git; download is an export artifact, not the only copy. Once persistence was corrected, user separately confirmed wanting real point-in-time revert, not just current-state-only.
- **Reason/rationale:** Matches the product's differentiating "safe to iterate" UX (CAP-006/History pattern in lovable-dev.md) — user can experiment via chat without fear of losing a good earlier state.
- **Alternatives considered:** Flat persisted state, no checkpoints/revert (simpler, less capability); no server-side persistence at all, re-upload zip to resume (rejected outright as bad UX, not a real option).
- **Why alternatives were rejected:** Flat-no-revert was the original MVP-lazy default but user explicitly wants point-in-time revert. No-persistence was a misreading of the original proposal, not something the user wanted.
- **Consequences/trade-offs:** Agent must produce clean, revertible commits per round (not just overwrite files) — more build effort than flat storage, but decided as worth it from the start rather than retrofitted later.
- **Status:** Active
- **Supersedes:** the flat-file MVP option floated in-chat 2026-09-13 was never formally recorded as a decision, so nothing to mark superseded — this is the first formal entry on file storage/versioning.

### D-5: Agent LLM = Claude Agent SDK
- **Date/session:** 2026-09-13
- **Decision:** Use Claude Agent SDK to drive the build agent (plans steps, writes/edits files, calls tools), not a hand-rolled Messages API tool-calling loop.
- **Context:** Core loop needs step-by-step interactive chat where user describes needs and agent builds incrementally, steerable mid-task (matches CAP-001/CAP-006 pattern from lovable-dev.md).
- **Reason/rationale:** Agent SDK already provides multi-turn conversation state, plan→tool→execute→observe loop, streaming output, and permission/hook callbacks — fits the consent-gate (CAP-007) and interrupt (CAP-006) patterns directly. Avoids reinventing conversation/retry/streaming/interrupt handling.
- **Alternatives considered:** Raw Claude Messages API with tool use, hand-rolled loop.
- **Why alternatives were rejected:** More control but requires building conversation state, retries, streaming, interrupt handling from scratch — all already solved by Agent SDK.
- **Consequences/trade-offs:** Less low-level control over the loop internals; depends on Agent SDK's abstractions and release cadence.
- **Status:** Active

### D-4: Build/preview compute = server-side sandbox (not in-browser WebContainers, not static-only)
- **Date/session:** 2026-09-13
- **Decision:** Generated apps build and run inside a server-side sandbox (Docker/VM per project), not purely in-browser WebContainers, not static-HTML-only.
- **Context:** Platform must produce a real, deployable/downloadable working app (D-2), not just a live preview.
- **Reason/rationale:** In-browser WebContainers can't produce a real deployable build artifact for "download and deploy elsewhere" in a way that matches a genuine build pipeline; static-HTML-only is too limited (no framework, no npm ecosystem). Server-side sandbox running real toolchains (npm/Vite) produces an actual deployable artifact.
- **Alternatives considered:** In-browser WebContainers (StackBlitz-style, like Bolt.new); static HTML/CSS/JS only, iframe srcdoc preview.
- **Why alternatives were rejected:** WebContainers = preview trick, not a real deploy build step. Static-only can't support real component frameworks or npm packages.
- **Consequences/trade-offs:** Must build/own container lifecycle, resource limits, idle teardown, isolation/security — real infra to build and maintain (see HBR-2/HBR-4 in `documents/lovable-dev.md` §21 for what this implies).
- **Status:** Active

### D-3: Output stack = React + Vite
- **Date/session:** 2026-09-13
- **Decision:** Generated web apps are built with React + Vite.
- **Context:** Need to pick what the codegen agent actually writes.
- **Reason/rationale:** Component-based, matches Lovable's own approach (per lovable-dev.md evidence), large npm ecosystem for the agent to draw on, standard `npm run build` produces static deployable output.
- **Alternatives considered:** Plain HTML/CSS/JS (no build step); let user pick framework per project.
- **Why alternatives were rejected:** Plain HTML too limited for real apps (no component reuse/npm ecosystem). Per-project framework choice adds codegen-prompt complexity not needed for first slice.
- **Consequences/trade-offs:** All generated apps locked to React/Vite for now; multi-framework support (if ever wanted) is future scope.
- **Status:** Active

### D-2: Deploy target = download-and-self-host first, one-click hosting deferred
- **Date/session:** 2026-09-13
- **Decision:** Platform builds the project and lets user download it (zip/dist) to self-host/test locally. One-click built-in hosting is a real end-goal but is deferred to a later sub-project, not built in the first slice.
- **Context:** Full product vision (per user) is to eventually offer both download AND hosting as user-facing options. Building both at once conflicts with keeping the first slice small.
- **Reason/rationale:** Download-only avoids building a hosting subsystem (CDN, domain routing, SSL, scaling) up front — that's a large independent subsystem in its own right. Confirmed by user: hosting remains part of the eventual product vision, just not phase 1.
- **Alternatives considered:** One-click hosting built in from the start; both simultaneously.
- **Why alternatives were rejected:** Both require building hosting infra before the core generation loop is even proven — inflates first slice.
- **Consequences/trade-offs:** Early users must self-host to actually run the deployed app; hosting subsystem is explicit future work, needs its own design pass later.
- **Status:** Active

### D-1: First slice = web app only, core loop only
- **Date/session:** 2026-09-13
- **Decision:** Scope the first build to: web apps only (no Android/mobile), core loop only (prompt → generate → live preview → chat-based iterate), per the MVP slice identified in `documents/lovable-dev.md` §18.
- **Context:** Full Lovable-equivalent platform spans many independent subsystems (agent+codegen, live preview infra, managed backend, billing, third-party integrations, security scanning, SEO tooling, collaboration, template gallery). User initially assumed Android support existed in Lovable's own product; verified via grep against `documents/lovable-dev.md` — zero mentions of Android/APK/mobile; report states "web-based" explicitly. User confirmed correction: web app only.
- **Reason/rationale:** Full platform is too large for one spec/build. Per brainstorming skill's decomposition guidance, scope down to the smallest coherent value-delivering slice and treat everything else as later sub-projects.
- **Alternatives considered:** Full platform at once; core loop + managed backend; Android-first or Android+web simultaneously.
- **Why alternatives were rejected:** Full platform = unbounded scope for a solo multi-session project. Android requires an entirely separate toolchain (Gradle/Android SDK/APK signing) with zero infra overlap with web — doing both at once doubles first-slice scope for no shared benefit.
- **Consequences/trade-offs:** Managed backend (DB/auth), billing, integrations, security scanning, SEO tooling, and Android support are all explicitly out of scope for now — tracked as future sub-projects, not abandoned.
- **Status:** Active

---
*When superseding a decision: do not edit the old entry's Decision/Context/Reason fields. Only change its Status field to `Superseded by D-<N>`, and add the new entry below/above per chronological order.*
