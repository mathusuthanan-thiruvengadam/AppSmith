# CLAUDE.md — AppSmith Project Instructions

This project is developed gradually across many separate Claude Code sessions — possibly weeks or months apart, across restarts, reboots, and new sessions. **The conversation history is not persistent project memory.** The `project-memory/` directory in this repo root is the persistent, human-readable source of project knowledge. Treat it as such.

## Project-memory files

| File | Purpose |
|---|---|
| `project-memory/PROJECT.md` | Current high-level understanding: purpose, problem, users, vision, direction, status, constraints, assumptions |
| `project-memory/REQUIREMENTS.md` | Confirmed and tentative requirements (functional/non-functional/constraints), plus a record of requirements that changed over time |
| `project-memory/DECISIONS.md` | Every significant decision: context, rationale, alternatives rejected, trade-offs, current status |
| `project-memory/BRAINSTORMING.md` | Brainstorming discussions and conclusions, including ideas not yet formalized as requirements or decisions |
| `project-memory/OPEN-QUESTIONS.md` | Unresolved questions and undecided matters |
| `project-memory/IDEAS.md` | Postponed-but-potentially-useful ideas, kept so they aren't lost |
| `project-memory/SESSION-LOG.md` | Chronological log of significant sessions: objective, topics, conclusions, decisions, requirements, ideas, open questions, work completed, next steps |

## Rules for every session

1. **Before doing significant project work**, read the relevant project-memory files first — at minimum `PROJECT.md`, `DECISIONS.md`, and `OPEN-QUESTIONS.md` — to reconstruct current project state. Do not rely solely on this conversation's history.
2. **Before proposing a major decision**, check `DECISIONS.md` for a prior decision on the same topic. If one exists, work from it rather than re-deciding from scratch.
3. **Never silently overwrite a decision.** When a decision changes: keep the old `DECISIONS.md` entry intact, mark its status as superseded, and add a new entry for the new decision that references the old one.
4. **Record as you go, not just at the end:**
   - Significant requirements → `REQUIREMENTS.md`
   - Significant decisions → `DECISIONS.md`
   - Useful brainstorming conclusions → `BRAINSTORMING.md`
   - Unresolved issues → `OPEN-QUESTIONS.md`
   - Postponed but useful ideas → `IDEAS.md`
5. **At the end of a significant session**, update `SESSION-LOG.md` with a new entry, and give the user a short "Project Memory Updated" summary: which files changed, what was captured.
6. **Don't log noise.** Not every exchange needs a project-memory entry — only things that could affect future understanding, architecture, requirements, decisions, or direction.
7. **If project-memory conflicts with the current conversation**, do not silently pick one. Surface the conflict to the user explicitly and ask, or clearly explain the discrepancy before proceeding.
8. **Keep files current and concise.** Update in place where appropriate (e.g. `PROJECT.md`'s status section), but never delete historical decision/requirement records merely to shorten a file — supersede, don't erase.
9. **When the user explicitly states** that something is a decision, requirement, important idea, constraint, or project fact, persist it in the correct file before the session ends — don't leave it only in chat.

## Scope note

This `CLAUDE.md` and `project-memory/` are project-local. They do not reference, modify, or depend on any global Claude Code skills/plugins configuration.
