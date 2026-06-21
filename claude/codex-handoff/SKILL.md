---
name: codex-handoff
description: Hand off unsupported, blocked, visual, browser, or review tasks from Claude Code to Codex CLI in a dual-agent workflow.
---

# Codex Handoff

Use this skill when Claude Code cannot reliably complete a subtask and Codex should analyze, review, or handle it.

## Handoff triggers

Invoke this skill when any of these occur:

- The task requires image, screenshot, UI, video, audio, or complex binary understanding.
- Claude lacks a required MCP/tool, such as browser automation or web extraction.
- A command/tool fails twice without a clear safe fix.
- The task needs independent code review, architectural judgment, or risk assessment.
- The user asks to involve Codex, use Codex as project manager, or follow dual-agent workflow.
- `CLAUDE.md` says to call Codex for this type of work.

## Default image handoff command

For images or screenshots, run:

```powershell
codex exec "分析这张图片，提取文字、界面结构、关键信息和可能的问题，不要修改任何文件" --sandbox read-only --skip-git-repo-check --image "图片完整路径"
```

Important:

- Put the prompt before `--image`.
- Use the real absolute image path.
- Keep `--sandbox read-only` unless the user explicitly wants Codex to modify files.
- Keep `--skip-git-repo-check` when running outside a trusted Git project.

## Default project review command

For project review, run from the project root:

```powershell
codex exec "使用 dual-agent-orchestrator 的项目经理视角，审查 PROJECT.md、TASKS.md、STATUS.md、BLOCKERS.md、CODEX_REVIEW.md 和当前代码改动。输出问题、风险、下一步建议。不要修改文件，除非用户明确要求。" --sandbox read-only --skip-git-repo-check
```

## Default blocker analysis command

When blocked, run from the project root:

```powershell
codex exec "分析当前项目阻塞。读取 PROJECT.md、TASKS.md、STATUS.md、BLOCKERS.md、CLAUDE.md，判断 Claude 卡住原因，并给出可执行的恢复步骤。不要修改文件。" --sandbox read-only --skip-git-repo-check
```

## After Codex responds

1. Summarize Codex output.
2. Write the useful result to `STATUS.md`.
3. If the issue remains unresolved, write it to `BLOCKERS.md`.
4. Continue only if Codex output gives a safe next step.

## Safety rules

- Do not send API keys, passwords, tokens, cookies, or private credentials to Codex.
- Do not allow broad shell permissions such as unrestricted `Bash(*)` just to run Codex.
- Prefer read-only Codex calls for analysis and review.
- Ask the user before allowing Codex or Claude to modify project files through a handoff.

## Command guardrails

Use these exact patterns to avoid prior mistakes:

- Put the natural-language prompt immediately after `codex exec`.
- Put flags after the prompt.
- Use an absolute path for images.
- Add `--skip-git-repo-check` when not running inside the project root.
- Use `--sandbox read-only` for analysis/review/image recognition.

Correct:

```powershell
codex exec "分析这张图片，提取文字、界面结构、关键信息和可能的问题，不要修改任何文件" --sandbox read-only --skip-git-repo-check --image "C:\full\path\image.png"
```

Incorrect:

```powershell
codex exec --sandbox read-only --image "C:\full\path\image.png" "分析这张图片"
```

If Codex CLI still fails, write the exact error to `BLOCKERS.md` and stop instead of retrying blindly.
