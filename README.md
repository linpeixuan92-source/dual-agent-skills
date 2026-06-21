# Dual Agent Skills

用于把 Codex 与 Claude Code/DeepSeek 组织成双代理协作流程的一组轻量 skill。

## 目标

这组 skill 解决几个实际问题：

- Codex 负责项目经理、需求拆解、视觉/图片理解、代码审查和最终验收。
- Claude Code + DeepSeek 负责执行代码修改、文档更新、Git 命令和进度汇报。
- 当 Claude 无法识别图片、缺少浏览器/MCP、命令卡住或输出质量不足时，按规则交接给 Codex。
- 通过 `PROJECT.md`、`TASKS.md`、`STATUS.md`、`BLOCKERS.md`、`CODEX_REVIEW.md`、`CLAUDE.md` 保持项目状态可追踪。

## 目录结构

```text
codex/
  dual-agent-orchestrator/
    SKILL.md
    agents/openai.yaml

claude/
  project-executor/
    SKILL.md
  status-reporter/
    SKILL.md
  codex-handoff/
    SKILL.md
```

## 安装到 Codex

把 Codex skill 复制到：

```powershell
C:\Users\<你的用户名>\.codex\skills\dual-agent-orchestrator
```

## 安装到 Claude Code

把 Claude skills 复制到：

```powershell
C:\Users\<你的用户名>\.claude\skills\project-executor
C:\Users\<你的用户名>\.claude\skills\status-reporter
C:\Users\<你的用户名>\.claude\skills\codex-handoff
```

安装或更新后，重新启动 Codex / Claude Code 会话，让 skill 重新加载。

## 推荐用法

### 1. Codex 初始化项目

对 Codex 说：

```text
使用 dual-agent-orchestrator 初始化当前项目。
Codex 负责拆任务和验收，Claude 负责执行。
```

Codex 会准备：

```text
PROJECT.md
TASKS.md
STATUS.md
BLOCKERS.md
CODEX_REVIEW.md
CLAUDE.md
```

### 2. Claude 执行

在 Claude Code 中运行：

```text
/project-executor /status-reporter /codex-handoff
```

然后让 Claude 读取 `CLAUDE.md`、`PROJECT.md`、`TASKS.md` 并执行。

### 3. Claude 卡住时交接给 Codex

如果 Claude 无法处理图片、浏览器、复杂判断或命令失败两次，应使用 `codex-handoff` 调用 Codex CLI，或把问题写入 `BLOCKERS.md` 后停止。

## 关键防呆规则

- 不要把 `C:\Windows\system32` 或用户主目录当成项目根目录。
- `Claude Code + DeepSeek` 应视为一个执行组合：Claude Code 负责工具执行，DeepSeek 负责模型推理。
- 网站和界面类任务由 Codex 定设计方向和验收标准，Claude 只按任务执行。
- 前端页面必须检查桌面端和约 390px 手机端。
- GitHub Pages 发布后要验证仓库地址、Pages 地址，以及 CSS/JS 是否更新。
- Codex CLI 识图命令中，prompt 必须放在 `--image` 前面。
- Claude 卡住两次后必须写入 `BLOCKERS.md`，不要继续盲试。

## License

MIT
