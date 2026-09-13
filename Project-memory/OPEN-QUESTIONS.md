# OPEN-QUESTIONS.md — AppSmith Open Questions

> Unresolved questions, uncertainties, things requiring investigation, and decisions not yet finalized. When a question resolves, move it (don't delete) to "Resolved Questions" with the answer and date, and if it produced a decision, link to DECISIONS.md.

**Last updated:** 2026-09-13

## Open

- Q: Which specific managed auth provider — Supabase Auth, Auth.js, or Clerk? All three flows were explained to the user (see BRAINSTORMING.md); user has not yet picked one. Raised 2026-09-13.
- Q: Which sandbox tech for server-side build (Docker vs Firecracker vs other), and where does it run (local dev machine vs cloud)? Raised 2026-09-13.
- Q: What exactly does "download" package — zip of source, zip of built dist, or both? Raised 2026-09-13.
- Q: If Supabase (or another provider that also offers a DB) is chosen, will AppSmith's own platform data (accounts, project metadata, git repo pointers) live in that same DB, or a separate one? Raised 2026-09-13 (surfaced while comparing providers).

## Resolved

### Q: How should users sign in — which auth methods?
- **Raised:** 2026-09-13
- **Resolved:** 2026-09-13
- **Answer:** Both email/password and OAuth supported.
- **Resulting decision:** DECISIONS.md#D-7

### Q: Build auth ourselves or use a managed provider?
- **Raised:** 2026-09-13
- **Resolved:** 2026-09-13
- **Answer:** Managed provider (specific provider still open — see Open section above).
- **Resulting decision:** DECISIONS.md#D-8

### Q: How is per-project file state stored/versioned?
- **Raised:** 2026-09-13
- **Resolved:** 2026-09-13
- **Answer:** Real git repo per project, server-side persisted per account, commit-per-round, revert-to-checkpoint supported.
- **Resulting decision:** DECISIONS.md#D-6

_Format for future resolutions:_

```
### Q: <question>
- **Raised:** YYYY-MM-DD
- **Resolved:** YYYY-MM-DD
- **Answer:** ...
- **Resulting decision:** DECISIONS.md#D-<number> (if applicable)
```

---
*Cross-reference: [DECISIONS.md](DECISIONS.md), [BRAINSTORMING.md](BRAINSTORMING.md)*
