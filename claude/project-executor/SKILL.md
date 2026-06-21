---
name: project-executor
description: Execute a project from repository coordination files. Use when the user asks Claude Code to start work from PROJECT.md, TASKS.md, CLAUDE.md, or a Codex-managed dual-agent workflow.
---

# Project Executor

Use this skill when starting or continuing work in a Codex-managed project.

## Required startup sequence

1. Identify the current project root.
2. Read these files if present:
   - `CLAUDE.md`
   - `PROJECT.md`
   - `TASKS.md`
   - `STATUS.md`
   - `BLOCKERS.md`
   - `CODEX_REVIEW.md`
3. Treat `CLAUDE.md` as the local operating rules.
4. Treat `PROJECT.md` as the project goal and constraints.
5. Treat `TASKS.md` as the active task queue.
6. Treat `CODEX_REVIEW.md` as higher-priority review feedback when it exists.

## Execution rules

- Work from `TASKS.md` in order unless the user explicitly overrides the priority.
- Do not invent requirements that are not in `PROJECT.md`, `TASKS.md`, or the user's latest message.
- Before making broad changes, summarize the intended change briefly.
- Keep changes narrow and directly tied to the active task.
- Prefer small, verifiable steps over large opaque changes.
- After each meaningful step, update `STATUS.md`.
- If blocked, update `BLOCKERS.md` and stop guessing.

## Safety boundaries

- Do not expose API keys, tokens, passwords, cookies, or credentials in chat or project files.
- Do not run destructive commands unless the user explicitly approves.
- Do not modify files outside the project unless the user explicitly asks.
- If Codex owns review or final validation, do not mark the task complete until Codex review feedback has been addressed.

## Completion format

When a task is complete:

1. Update `STATUS.md` with:
   - task completed
   - files changed
   - commands run
   - verification result
2. If anything remains uncertain, write it to `BLOCKERS.md`.
3. Tell the user what changed and what should be reviewed by Codex.

## Known failure-prevention rules

- Confirm the project root before editing. Do not work from `C:\Windows\system32` or the user home directory unless the user explicitly says so.
- In this workflow, `Claude Code + DeepSeek` is one execution stack: Claude Code runs tools and commands; DeepSeek may be the configured reasoning/model backend.
- For visual website work, follow Codex's design direction from `PROJECT.md`, `TASKS.md`, or `CODEX_REVIEW.md`; do not invent a different visual system unless asked.
- For GitHub Pages, ensure `index.html`, `styles.css`, and `script.js` are committed and pushed to the selected branch. If Pages is not enabled, report the exact manual setting needed.
- Before marking frontend work complete, verify at least desktop and 390px mobile layouts. Check for horizontal overflow, clipped navigation, oversized buttons/cards, and unreadable text.
- If authentication, browser authorization, GitHub permissions, or Pages settings block progress, write the exact blocker to `BLOCKERS.md` instead of guessing.
- Do not modify `CODEX_REVIEW.md` unless the user explicitly asks. Codex owns final review.
