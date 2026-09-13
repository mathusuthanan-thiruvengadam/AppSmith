# IDEAS.md — AppSmith Idea Backlog

> Potentially useful ideas that are NOT currently part of the implementation plan. Postponed, not rejected — keep them so good ideas don't get lost just because they came up too early or out of scope.

**Last updated:** 2026-09-13 (template only, no ideas recorded yet)

## How to log an idea

```
### I-<number>: <short title>
- **Date/session:** YYYY-MM-DD
- **Idea:** description
- **Why postponed:** not needed yet / out of current scope / depends on X / etc.
- **Revisit when:** trigger condition, if known
```

## Backlog

### I-1: One-click hosting subsystem
- **Date/session:** 2026-09-13
- **Idea:** Platform hosts generated apps directly (own domain/subdomain, CDN, SSL) as an alternative to download-and-self-host.
- **Why postponed:** Large independent subsystem (hosting infra) — building it before the core generation loop is proven inflates first slice. See DECISIONS.md D-2.
- **Revisit when:** Core loop (D-1) is working end-to-end and download-based deploy is validated.

---
*Cross-reference: an idea graduates into REQUIREMENTS.md or DECISIONS.md once actually adopted — leave a note here pointing to where it went, don't just delete it.*
