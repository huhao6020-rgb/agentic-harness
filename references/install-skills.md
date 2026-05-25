# 安装与初始化

## 原则

初始化先选平台，再装对应能力。不要为了“以后可能会用”同时创建 Codex 和 Claude Code 的目录。

```text
确认平台
-> 检查项目级 skills
-> 检查 CLI / 插件
-> 记录 skill-manifest.md
-> 缺失则询问安装或标记 pending
```

外部安装、全局 CLI、hooks、`.codex/config.toml`、`.claude/settings.json` 都必须单独确认。

本文件服务于 `bootstrap-phases.md` 的阶段二和阶段三。初始化时必须把每个检查结果写入 `docs/agentic-dev/bootstrap-report.md` 和 `docs/agentic-dev/skill-manifest.md`。

## 平台选择

| 用户选择 | 创建内容 | 不创建 |
| --- | --- | --- |
| Codex | `AGENTS.md`、`.agents/skills/` | `.claude/` |
| Claude Code | `CLAUDE.md`、`.claude/skills/` | `.agents/` |
| 双平台 | `AGENTS.md`、`CLAUDE.md`、`.agents/skills/`、`.claude/skills/` | 无 |

双平台时，`AGENTS.md` 作为公共规则入口，`CLAUDE.md` 第一行导入 `@AGENTS.md`。

## 项目级 skill 清单

| 名称 | 类型 | Codex 位置 | Claude Code 位置 | 规则 |
| --- | --- | --- | --- | --- |
| `ui-ux-pro-max` | 项目级 skill | `.agents/skills/ui-ux-pro-max/` | `.claude/skills/ui-ux-pro-max/` | UI/UX 任务使用 |
| `graphify` | CLI + 项目级 skill | `.agents/skills/graphify/` | `.claude/skills/graphify/` | 项目理解层 |
| `prd-test-writer` | 项目级 skill | `.agents/skills/prd-test-writer/` | `.claude/skills/prd-test-writer/` | 深聊对齐和测试基准 |

不写入项目级依赖：

- `agent-reach`：只作为当前运行环境可用的外部搜索能力。
- Superpowers：作为 Codex 插件/全局能力使用，不复制成项目 skill。
- OpenSpec / Spec-Kit：通过官方 CLI 初始化，不手写伪造 skill。

## 项目级 skill 验收标准

Codex 项目级 skill 成功标准：

```text
.agents/skills/<skill-name>/SKILL.md
```

硬规则：`.agents/skills/<skill-name>/SKILL.md` 存在才算 Codex 项目级 skill installed。

Claude Code 项目级 skill 成功标准：

```text
.claude/skills/<skill-name>/SKILL.md
```

硬规则：`.claude/skills/<skill-name>/SKILL.md` 存在才算 Claude Code 项目级 skill installed。

以下情况都不能算安装成功：

- 只有 `.agents/skills/README.md`。
- 只有 `.claude/skills/README.md`。
- README.md 只算说明文件，不算 skill 安装成功。
- 只有空目录。
- 只有 CLI 可用但项目级 `SKILL.md` 不存在。
- 从全局目录“看起来可用”，但项目目录没有对应 skill，且本项目要求项目级安装。

发现 README-only 时，必须写入：

```text
状态：failed 或 pending
原因：只有 README，不存在 <skill-name>/SKILL.md
下一步：安装、复制完整 skill，或由用户确认跳过
```

## `skill-manifest.md`

项目初始化时创建：

```text
docs/agentic-dev/skill-manifest.md
```

最小内容：

```md
# Skill Manifest

## 平台
- 当前平台：Codex / Claude Code / 双平台
- 主平台：

## 项目级 Skills
| 名称 | 状态 | 来源 | 项目路径 | 验证方式 | 备注 |
| --- | --- | --- | --- | --- | --- |
| ui-ux-pro-max | pending | nextlevelbuilder/ui-ux-pro-max-skill | `.agents/skills/ui-ux-pro-max/SKILL.md` 或 `.claude/skills/ui-ux-pro-max/SKILL.md` | 检查项目级 `SKILL.md` |  |
| graphify | pending | safishamsi/graphify | `.agents/skills/graphify/SKILL.md` 或 `.claude/skills/graphify/SKILL.md` | `graphify --version` + 检查项目级 `SKILL.md` |  |
| prd-test-writer | pending | 本机/用户提供来源 | `.agents/skills/prd-test-writer/SKILL.md` 或 `.claude/skills/prd-test-writer/SKILL.md` | 检查项目级 `SKILL.md` |  |

## CLI / 规范系统
| 名称 | 状态 | 验证方式 | 初始化命令 | 备注 |
| --- | --- | --- | --- | --- |
| Graphify | pending | `graphify --version` | `graphify install --project --platform <platform>` |  |
| Spec-Kit | pending | `specify version` | `specify init ...` | 场景三默认 |
| OpenSpec | pending | `openspec --version` | `openspec init --tools <tools>` | 场景一/二按需 |
```

状态只允许：`installed`、`pending`、`skipped`、`blocked`、`failed`。

## ui-ux-pro-max

来源：`nextlevelbuilder/ui-ux-pro-max-skill`。

推荐项目级安装：

```bash
npm install -g uipro-cli
cd <project-root>
uipro init --ai codex
uipro init --ai claude
```

按平台只执行对应命令。安装后验证目标路径是否存在；如果 CLI 生成的路径与平台官方约定不一致，记录实际路径到 `skill-manifest.md`，不要静默搬运。

## Graphify

来源：`safishamsi/graphify`。

官方包名是 `graphifyy`，命令是 `graphify`。

```bash
uv tool install graphifyy
# 或
pipx install graphifyy

graphify --version
```

项目级安装：

```bash
graphify install --project --platform codex
graphify install --project --platform claude
```

也可以使用平台子命令：

```bash
graphify codex install --project
graphify claude install --project
```

生成图谱：

```bash
graphify .
```

输出：

```text
graphify-out/
  graph.html
  GRAPH_REPORT.md
  graph.json
```

如果项目还没有源码或文档骨架，不强行运行 Graphify，只在 `graphify.md` 写明“待基础内容出现后生成”。

## prd-test-writer

`prd-test-writer` 当前没有统一公开安装命令，本 harness 只支持“有则复制，缺则提示”。

检查来源：

```text
<用户主目录>/.codex/skills/prd-test-writer/
<用户主目录>/.agents/skills/prd-test-writer/
<用户主目录>/.claude/skills/prd-test-writer/
```

如果存在完整 `SKILL.md`，按平台复制完整目录：

```text
Codex -> .agents/skills/prd-test-writer/
Claude -> .claude/skills/prd-test-writer/
```

如果不存在：

- 不创建空目录。
- 不生成伪造 `SKILL.md`。
- 在 `skill-manifest.md` 标记 `pending`。
- 提示用户先安装或提供来源。

## Spec-Kit

来源：`github/spec-kit`。

安装：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
specify version
```

场景三默认使用 Spec-Kit。按平台初始化：

```bash
# Codex skills 模式
specify init . --integration codex --integration-options="--skills"

# Claude Code
specify init . --integration claude
```

双平台时先初始化主平台，再追加另一个平台：

```bash
specify integration install claude
specify integration install codex --integration-options="--skills"
```

以实际 CLI 输出为准，并记录到 `spec-system.md` 和 `skill-manifest.md`。

## OpenSpec

来源：`Fission-AI/OpenSpec`。

安装：

```bash
npm install -g @fission-ai/openspec@latest
openspec --version
```

场景一/二 medium 以上按需使用。按平台初始化：

```bash
openspec init --tools codex
openspec init --tools claude
openspec init --tools codex,claude
```

它会创建：

```text
openspec/
  specs/
  changes/
  config.yaml
```

并按工具生成 OpenSpec skills / commands。不要手写这些目录；实际生成路径写入 `spec-system.md` 和 `skill-manifest.md`。

## 失败处理

任何安装或初始化失败时：

1. 记录命令、错误摘要、当前状态。
2. 写入 `skill-manifest.md`、`bootstrap-report.md` 和相关状态文件。
3. 不声称已安装或已初始化。
4. 询问用户是否安装依赖、换平台、跳过或降级。
