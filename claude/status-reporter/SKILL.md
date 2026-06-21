---
name: status-reporter
description: Maintain STATUS.md and BLOCKERS.md during project execution. Use when the user wants Claude Code to report progress, avoid silent stalls, or coordinate with Codex review.
---

# Status Reporter

Use this skill whenever work spans more than one step, uses tools, edits files, runs commands, or may block.

## Reporting files

Use these files in the project root:

- `STATUS.md` for progress and completed work.
- `BLOCKERS.md` for unresolved issues, missing permissions, missing tools, failed commands, or unclear requirements.

Create the files if they are missing.

## STATUS.md format

Append concise entries using this structure:

```md
## YYYY-MM-DD HH:mm - Status

- Active task:
- Completed:
- Files changed:
- Commands run:
- Verification:
- Next step:
```

## BLOCKERS.md format

Append blockers using this structure:

```md
## YYYY-MM-DD HH:mm - Blocker

- Task:
- Problem:
- Evidence:
- What I tried:
- Needed from user/Codex:
```

## When to update

Update `STATUS.md`:

- after reading the project coordination files
- before starting a risky or multi-step change
- after each meaningful completed step
- after running tests or verification
- before handing work back to Codex or the user

Update `BLOCKERS.md`:

- when a command fails and there is no safe obvious fix
- when a required MCP/tool/permission is missing
- when a file, credential, API key, or dependency is unavailable
- when the task is ambiguous enough that guessing could cause wrong work
- when Claude cannot process an input type, such as an image, video, audio, or complex binary file

## Stall prevention rule

If no useful progress is possible after two attempts, stop the current approach, write the blocker, and ask for Codex/user handoff instead of continuing silently.

## Chat behavior

Keep chat updates short. Put detailed progress in `STATUS.md` and detailed failures in `BLOCKERS.md`.

## Required blocker details for common failures

When blocked, include concrete evidence rather than a generic failure:

- Git/GitHub: include the command, error text, branch, remote URL, and whether authentication was requested.
- GitHub Pages: include repo URL, Pages URL if known, selected source branch/folder, and whether the page returns HTTP 200.
- Frontend layout: include which viewport failed, e.g. desktop or 390px mobile, and the visible symptom such as clipped nav, horizontal overflow, or unreadable cards.
- Codex CLI handoff: include the exact command attempted. If the error says `No prompt provided`, the prompt was likely missing or placed incorrectly. If it says `Not inside a trusted directory`, add `--skip-git-repo-check`.
- Missing capability: state whether the missing item is a model limitation, MCP/tool limitation, permission issue, or missing credential.

If the same operation has failed twice, stop and request Codex/user handoff.
