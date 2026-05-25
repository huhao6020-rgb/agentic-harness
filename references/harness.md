# Harness 结构

## 四层

- 平台入口层：`AGENTS.md` 或 `CLAUDE.md`。
- 状态层：`docs/agentic-dev/`。
- 需求层：`docs/prd/` 和 `docs/agentic-dev/prd-gate.md`。
- 记忆层：`memory/`。

## 平台入口层

Codex:

```text
AGENTS.md
.agents/skills/
```

Claude Code:

```text
CLAUDE.md
.claude/skills/
```

双平台:

```text
AGENTS.md
CLAUDE.md
.agents/skills/
.claude/skills/
```

## 状态层

```text
docs/agentic-dev/
  INDEX.md
  ACTIVE.md
  project-structure.md
  development-protocol.md
  graphify.md
  spec-system.md
  skill-manifest.md
  prd-gate.md
  decisions.md
```

## 需求层

```text
docs/prd/
docs/PRD_REGISTRY.md
```

需求层由 `prd-test-writer` 或等价深聊流程维护，是 Spec-Kit / OpenSpec / tasks 的上游事实源。

## 记忆层

```text
memory/
  project.md
  decisions.md
  constraints.md
  glossary.md
  modules/
```

## 不属于 harness 的内容

- `.codex/agentic-dev/`
- `.codex/goals/`
- `.codex/tasks/`
- `.uv-cache`
- `.pytest_cache`
- `node_modules`
- `.playwright-mcp`
