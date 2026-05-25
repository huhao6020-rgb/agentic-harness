# Claude Code 适配

## 平台入口

Claude Code 项目使用 `CLAUDE.md`。不要在用户选择 Claude-only 时强制创建 `.agents/` 或 Codex 专属文件。

Claude-only 项目：

```md
# 项目智能体规则

## 平台
- 当前平台：Claude Code
- 项目级 skills：`.claude/skills/`
- 状态入口：`docs/agentic-dev/INDEX.md`

## 启动规则
- 先读 `docs/agentic-dev/INDEX.md` 和 `ACTIVE.md`。
- 检查 `prd-gate.md`、`spec-system.md`、`graphify.md`、`skill-manifest.md`。
- medium / large / greenfield 未完成深聊对齐，不得进入实现。
- 未验证不得声称完成。
```

双平台项目：

```md
@AGENTS.md

## Claude Code 适配
- 公共规则以 `AGENTS.md` 为准。
- Claude Code 项目级 skills 放 `.claude/skills/`。
- Claude Code 专属 hooks、agents、commands 放 `.claude/`。
- 不复制公共规则，避免 `AGENTS.md` 与 `CLAUDE.md` 漂移。
```

## 目录边界

- `.claude/skills/`：项目级 Claude skills。
- `.claude/commands/`：Claude Code slash commands。
- `.claude/agents/`：Claude 子代理配置。
- `.claude/settings.json`：Claude Code 设置；修改前必须确认。

项目状态仍放 `docs/agentic-dev/`，长期事实放 `memory/`。

## 与 Codex 的关系

- Claude-only：不创建 `AGENTS.md` 和 `.agents/skills/`，除非用户要求迁移或双平台。
- 双平台：`AGENTS.md` 是公共规则，`CLAUDE.md` 只做适配。
- 不要让 `CLAUDE.md` 和 `AGENTS.md` 各自维护两套公共规则。

## 启动检查

1. 当前平台是否为 Claude Code 或双平台。
2. `.claude/skills/` 是否包含所需项目级 skills。
3. `prd-gate.md` 是否 aligned。
4. `spec-system.md` 是否选择唯一规范系统。
5. `ACTIVE.md` 是否有允许写入范围和验证标准。
