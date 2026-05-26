# 初始化阶段门禁

## 核心原则

初始化不是一次性生成一堆文件，而是按阶段完成、按阶段验收、按阶段报告。每次执行项目初始化时，必须展示“初始化阶段报告”，不能只用自然语言说“已完成初始化”。

本文件适用于：

- 新项目初始化。
- 第一次把 `agentic-harness` 接入现有项目。
- 用户要求“初始化当前项目开发框架”“搭建 harness”“构建 AGENTS.md / CLAUDE.md”。

## 阶段报告位置

初始化过程必须创建或更新：

```text
docs/agentic-dev/bootstrap-report.md
```

阶段状态只允许：

```text
todo / running / done / pending / blocked / failed / skipped
```

## 阶段报告模板

```md
# Agentic Harness 初始化阶段报告

## 总览
- 当前平台：
- 主平台：
- 初始化状态：running / done / pending / blocked / failed
- 当前阶段：
- 下一步：

## 阶段表
| 阶段 | 名称 | 状态 | 产物 | 验收结果 | 下一步 |
| --- | --- | --- | --- | --- | --- |
| 1 | 平台确认 |  |  |  |  |
| 2 | 依赖与 skill 检查 |  |  |  |  |
| 3 | 项目级 skill 安装 |  |  |  |  |
| 4 | 项目框架构建 |  |  |  |  |
| 5 | 结构化入口规则构建 |  |  |  |  |
| 6 | 规范与图谱状态初始化 |  |  |  |  |
| 7 | 验证与交接 |  |  |  |  |

## Pending / Blocked
| 项目 | 原因 | 用户需要决定什么 |
| --- | --- | --- |

## 验证证据
- 文件结构：
- 项目级 skills：
- CLI：
- 入口规则：
- 下一轮入口：
```

## 阶段一：平台确认

输入：

- 用户请求。
- 当前项目是否已有 `AGENTS.md` / `CLAUDE.md` / `.agents/` / `.claude/`。

动作：

- 询问或判断目标平台：Codex、Claude Code、双平台。
- 如果用户未明确平台，停止询问，不创建平台目录。
- 双平台时确认主平台。

产物：

- `bootstrap-report.md` 记录平台和主平台。

验收：

- Codex：后续只允许创建 `AGENTS.md` 和 `.agents/skills/`。
- Claude Code：后续只允许创建 `CLAUDE.md` 和 `.claude/skills/`。
- 双平台：允许两套入口，但必须写明主平台。

失败处理：

- 平台不明确：阶段状态写 `blocked`，不进入阶段二。
- 用户要求项目内 `.codex/skills`：说明 Codex 官方项目级 skills 是 `.agents/skills`，不是项目内 `.codex/skills`；`.codex/` 只用于官方 config/hooks。

## 阶段二：依赖与 skill 检查

输入：

- 阶段一确认的平台。
- `references/install-skills.md`。

动作：

- 检查项目级 skills：`ui-ux-pro-max`、`graphify`、`prd-test-writer`。
- 检查 CLI：Graphify、Spec-Kit、OpenSpec。
- 检查 Superpowers 是否作为当前环境能力可用；不复制为项目 skill。
- 写入 `docs/agentic-dev/skill-manifest.md`。

产物：

```text
docs/agentic-dev/skill-manifest.md
docs/agentic-dev/bootstrap-report.md
```

验收：

- 每个 skill / CLI 都有状态：`installed`、`pending`、`skipped`、`blocked`、`failed`。
- 不存在的能力不能写成 installed。

失败处理：

- CLI 不存在：写 `pending` 或 `blocked`，记录安装命令。
- 需要联网或全局安装：先请求用户确认。

## 阶段三：项目级 skill 安装

输入：

- 阶段二的 `skill-manifest.md`。
- 用户是否允许安装、复制或跳过。

动作：

- Codex 项目级 skills 安装到 `.agents/skills/<skill-name>/`。
- Claude Code 项目级 skills 安装到 `.claude/skills/<skill-name>/`。
- `prd-test-writer` 有本机来源则复制完整目录；无来源则 pending。
- `ui-ux-pro-max` 和 Graphify 优先按官方 CLI 初始化；失败写 pending。

产物：

```text
.agents/skills/<skill-name>/SKILL.md
.claude/skills/<skill-name>/SKILL.md
docs/agentic-dev/skill-manifest.md
```

验收：

- Codex skill 成功 = `.agents/skills/<skill-name>/SKILL.md` 存在。
- Claude skill 成功 = `.claude/skills/<skill-name>/SKILL.md` 存在。
- `.agents/skills/README.md` 或 `.claude/skills/README.md` 只算说明文件，不算任何 skill 安装成功。
- 硬规则：`.agents/skills/<skill-name>/SKILL.md` 才是 Codex 项目级 skill 安装成功标志。
- 硬规则：README.md 只算说明文件，不算 skill 安装成功。

失败处理：

- 只有 README：阶段状态写 `failed` 或对应 skill 写 `pending`。
- 缺少 `SKILL.md`：不得声称安装完成。
- 用户暂不安装：写 `skipped`，并说明后续影响。

## 阶段四：项目框架构建

输入：

- 平台选择。
- 项目自定义规则采集结果。

动作：

- 创建平台无关状态层和记忆层。
- 创建 `bootstrap-report.md`、`skill-manifest.md`、`prd-gate.md`、`spec-system.md`、`graphify.md`。
- greenfield 或用户要求长跑时，创建 Task Queue Harness 入口。

产物：

```text
docs/agentic-dev/
  INDEX.md
  ACTIVE.md
  bootstrap-report.md
  project-structure.md
  development-protocol.md
  graphify.md
  spec-system.md
  skill-manifest.md
  prd-gate.md
  decisions.md
  TASKS.md
  task-runner.md
  handoff.md
  verification-matrix.md
  tasks/
    README.md
memory/
  project.md
  decisions.md
  constraints.md
  glossary.md
  modules/
```

验收：

- 必需文件存在且标题为简体中文。
- `project-structure.md` 是目录结构唯一来源。
- `ACTIVE.md` 写明当前阶段、下一步、允许写入范围、阻塞项。
- `TASKS.md` 写明任务状态规则；`task-runner.md` 写明执行循环；`handoff.md` 写明恢复入口。

失败处理：

- 缺任一必需文件：阶段状态写 `failed`，不进入阶段五。

## 阶段五：结构化入口规则构建

输入：

- `references/rules-and-memory.md`。
- 阶段四产物。
- 用户自定义规则。

动作：

- Codex 创建结构化 `AGENTS.md`。
- Claude Code 创建结构化 `CLAUDE.md`。
- 双平台时 `CLAUDE.md` 只导入 `@AGENTS.md` 并写 Claude 适配。
- 入口规则必须包含“初始化阶段报告”入口。
- 入口规则必须包含“任务队列长跑协议”入口。

产物：

```text
AGENTS.md
CLAUDE.md
```

验收：

- Codex `AGENTS.md` 写明项目级 skills 位于 `.agents/skills/`，不是 `.codex/skills`。
- 入口规则包含“项目自定义规则”区。
- 入口规则包含 `docs/agentic-dev/bootstrap-report.md`。
- 入口规则包含 `docs/agentic-dev/TASKS.md`、`task-runner.md`、`handoff.md`、`verification-matrix.md`。
- 入口规则不堆临时任务历史。

失败处理：

- 入口规则缺结构化框架：阶段状态写 `failed`。
- 用户自定义规则不明确：写入 `ACTIVE.md` 待确认，不伪造。

## 阶段六：规范与图谱状态初始化

输入：

- `skill-manifest.md`。
- `spec-system.md`。
- `graphify.md`。

动作：

- Graphify 可用且有可分析内容时运行生成图谱；否则记录待生成。
- 场景三默认 Spec-Kit，写明初始化状态。
- 场景一/二 medium 以上按需 OpenSpec。
- 不混用 OpenSpec 和 Spec-Kit。

产物：

```text
docs/agentic-dev/graphify.md
docs/agentic-dev/spec-system.md
graphify-out/
openspec/
.specify/ 或 specs/
```

验收：

- Graphify 状态明确：installed / pending / skipped / blocked / failed。
- 规范系统唯一。
- CLI 缺失时只写 pending / blocked，不伪造目录或成功状态。

失败处理：

- CLI 缺失：记录安装命令和下一步。
- 初始化失败：保留错误摘要，不进入业务代码。

## 阶段七：验证与交接

输入：

- 前六阶段产物。

动作：

- 检查必需文件。
- 检查项目级 skill 的 `SKILL.md`。
- 检查 `skill-manifest.md` 状态。
- 检查入口规则是否引用阶段报告、PRD 门禁、规范系统和 Graphify。
- 检查入口规则是否引用 Task Queue Harness，并声明 `ready` / `verify_failed` 不得整体完成。
- 输出最终阶段报告和下一步。

验收：

- `bootstrap-report.md` 阶段表完整。
- Codex 项目没有误建 `.codex/skills`。
- 只有 README 的 skill 目录不算 installed。
- pending / blocked 项清楚写明用户需要做什么。
- 长跑入口存在且可恢复：`TASKS.md`、`task-runner.md`、`handoff.md`、`verification-matrix.md`。

失败处理：

- 任一必需验收失败：初始化状态不得写 `done`。
- 最终回复必须说明失败阶段、原因、下一步。

## 最终回复格式

初始化完成或中断时，最终回复必须包含：

```md
## 初始化阶段报告

| 阶段 | 名称 | 状态 | 结果 |
| --- | --- | --- | --- |
| 1 | 平台确认 |  |  |
| 2 | 依赖与 skill 检查 |  |  |
| 3 | 项目级 skill 安装 |  |  |
| 4 | 项目框架构建 |  |  |
| 5 | 结构化入口规则构建 |  |  |
| 6 | 规范与图谱状态初始化 |  |  |
| 7 | 验证与交接 |  |  |

## Pending / Blocked

## 下一步
```

不得只输出“已完成初始化”。
