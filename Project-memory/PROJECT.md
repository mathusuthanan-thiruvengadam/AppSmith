# PROJECT.md — AppSmith Project Overview

> Current high-level understanding of the project. Update this file whenever the purpose, direction, or status changes. This is the "what and why" — see REQUIREMENTS.md for the "what exactly," DECISIONS.md for the "why this way."

**Last updated:** 2026-09-13

## Project Purpose

Build an AI app builder in the style of Lovable.dev (reverse-engineered in `documents/lovable-dev.md`): user describes a web app in natural language via chat, an AI agent generates it, live-previews it, and iterates on it conversationally.

## Problem Being Solved

TBD — not yet articulated beyond "build something like Lovable."

## Target Users

TBD — likely the project owner first (self-use / learning project), broader audience undefined.

## Project Vision

Full vision (stated by user): eventually give the user both options for a generated app — download it (zip/dist, self-host and test locally) AND have the platform host it directly if wanted. Hosting is real end-goal, not dropped, just deferred (see DECISIONS.md D-2).

## Current Direction

First slice (DECISIONS.md D-1): web app only (no Android/mobile), core loop only — prompt → generate → live preview → chat-based iterate. Stack: React + Vite output (D-3), server-side sandbox build (D-4), Claude Agent SDK driving the build agent (D-5), download-and-self-host deploy for now (D-2), server-side persistence per account with git-backed checkpoints/revert (D-6), AppSmith accounts with email/password + OAuth sign-in via a managed auth provider — specific provider TBD (D-7, D-8). Managed backend *for generated apps*, billing, integrations, security scanning, SEO tooling, collaboration, template gallery, and Android support are explicitly out of scope for this slice — see REQUIREMENTS.md.

## Current Status

- Stage: architectural brainstorming (in progress)
- Project-memory system created: 2026-09-13
- No code written yet
- Core-loop + account architecture decided (see DECISIONS.md D-1..D-8); open: which specific managed auth provider, sandbox tech specifics, download packaging format (see OPEN-QUESTIONS.md)

## Important Constraints

- Solo, multi-session project developed gradually over many separate Claude Code sessions — project-memory system exists specifically to survive that.
- Reference material: `documents/lovable-dev.md` is a reverse-engineered report on Lovable.dev, not a spec — used as inspiration/comparison, not ground truth for this project's own requirements.

## Important Assumptions

- Full Lovable-equivalent feature set is out of reach for a first build; scoping to MVP core loop per lovable-dev.md §18.
- "Similar to Lovable" means the interaction pattern (chat-driven, iterative, live preview) and real deployable output — not a literal feature-for-feature clone.

---
*See also: [REQUIREMENTS.md](REQUIREMENTS.md), [DECISIONS.md](DECISIONS.md), [OPEN-QUESTIONS.md](OPEN-QUESTIONS.md)*
