# 项目级 Bootstrap Contract

## 适用场景

用户提出“新建项目”“从零构建”“创建应用”“初始化项目框架”“先把 harness 搭起来”等请求时，进入场景三。此时必须先完成项目级 bootstrap 和深聊对齐，再进入业务代码、UI、接口或目录实现。

场景一的小 bug、文案、配置修复不使用本合约。场景二只在缺少项目入口规则时补齐必要部分，不创建完整新项目框架。

## Bootstrap 顺序

初始化必须按 `bootstrap-phases.md` 执行，不允许一次性生成文件后直接宣布完成。

```text
确认平台
-> 依赖与 skill 检查
-> 项目级 skill 安装
-> 创建平台入口规则
-> 创建 docs/agentic-dev 和 memory
-> 创建 skill-manifest / prd-gate / spec-system / graphify 状态
-> 深聊对齐
-> 初始化唯一规范系统
-> 验证 bootstrap
-> 再进入业务开发
```

## 平台入口

| 平台 | 必建入口 | 必建 skill 目录 | 不做 |
| --- | --- | --- | --- |
| Codex | `AGENTS.md` | `.agents/skills/` | 不创建 `.claude/` |
| Claude Code | `CLAUDE.md` | `.claude/skills/` | 不创建 `.agents/` |
| 双平台 | `AGENTS.md` + `CLAUDE.md` | `.agents/skills/` + `.claude/skills/` | 无 |

双平台时，`AGENTS.md` 是公共规则入口，`CLAUDE.md` 只导入 `@AGENTS.md` 并写 Claude Code 适配。Claude-only 项目可以只用 `CLAUDE.md`，不要强行创建 `AGENTS.md`。

## 平台无关框架

始终创建：

```text
docs/
  agentic-dev/
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

按需创建：

```text
docs/agentic-dev/plans/
docs/agentic-dev/tasks/
docs/agentic-dev/runs/
docs/agentic-dev/verification/
docs/prd/
docs/PRD_REGISTRY.md
```

`TASKS.md`、`task-runner.md`、`handoff.md`、`verification-matrix.md` 是 greenfield 或 long-running 初始化的默认产物。`plans/`、`runs/` 和额外验证目录只在确有需要时创建。

## Bootstrap 必写内容

- 初始化阶段报告：`bootstrap-report.md`。
- 当前平台与主平台。
- 项目目标、目标用户、当前阶段、不可接受结果。
- 项目自定义规则：目录结构、接口方式、UI 偏好、权限、数据源、禁止修改范围、交付偏好。
- 目录结构唯一来源：`project-structure.md`。
- 项目级 skill / CLI 状态：`skill-manifest.md`。
- 深聊对齐状态：`prd-gate.md`。
- Graphify 状态：已生成、待生成、需安装、用户暂缓或失败。
- 唯一规范系统：场景三默认 Spec-Kit；不得同时初始化 OpenSpec。
- 任务队列状态：`TASKS.md` 是唯一任务队列事实源；`task-runner.md` 是执行协议；`handoff.md` 是恢复入口。
- 完成标准：测试、构建、lint、浏览器验收、OpenSpec verify 或等价证据。

## 深聊门禁

新项目不得先写业务代码再补 PRD。顺序是：

```text
prd-test-writer 或等价深聊
-> PRD / 用户故事 / 验收标准 / 测试基准
-> Spec-Kit constitution / specify / plan / tasks
-> 业务实现
```

如果 `prd-test-writer` 不存在，记录到 `skill-manifest.md`，询问用户安装或允许手动降级；不得伪造 skill 或空目录。

## 自定义规则采集门禁

初始化入口规则前，必须向用户确认一次：

```text
这个项目有没有必须遵守的特殊规则？
例如目录结构、接口方式、UI 偏好、权限、数据源、禁止修改范围、交付偏好或不可接受结果。
```

用户回答后：

- 稳定规则写入 `AGENTS.md` / `CLAUDE.md` 的“项目自定义规则”。
- 较长事实写入 `memory/constraints.md`、`memory/project.md` 或相关模块记忆。
- 不确定是否稳定时，先写入 `ACTIVE.md` 的待确认项。

## Graphify 门禁

- 初始化必须创建 `graphify.md`。
- 如果 Graphify 已安装且项目已有基础源码或文档骨架，运行 `graphify .` 或平台命令生成 `graphify-out/`。
- 如果项目还没有可分析内容，记录“待基础内容出现后生成”。
- 如果 Graphify 不可用，写入 `graphify.md`、`ACTIVE.md` 和 `skill-manifest.md`，不声称已完成架构理解。

## Spec 门禁

- 场景三默认 Spec-Kit。
- 场景一/二 medium 以上按需 OpenSpec。
- 小任务不启规范系统。
- 规范系统必须写入 `spec-system.md`，包含选择原因、初始化状态、入口命令和禁止混用规则。

## 停止条件

遇到以下情况必须停下说明：

- 任一初始化阶段失败且没有明确 pending / blocked 处理。
- 项目级 skill 目录只有 README，没有 `<skill-name>/SKILL.md`。
- 用户目的、技术栈、成功标准或验收信号不清楚。
- 未确认平台。
- 需要安装 Graphify、Spec-Kit、OpenSpec、`ui-ux-pro-max` 或 `prd-test-writer`。
- 需要创建 hooks、修改 `.codex/config.toml`、`.claude/settings.json` 或全局配置。
- 目录结构无法确定。
- Graphify、Spec-Kit 或 OpenSpec 初始化失败。
- 没有任何验证命令或验收信号。
