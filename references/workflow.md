# 整体工作流

## 闭环

```text
平台选择
-> 规则与状态恢复
-> 深聊对齐
-> 三场景路由
-> 复杂度分级
-> 项目理解 / bootstrap
-> 规范系统
-> 执行
-> 验证
-> 记忆写回
```

## 每次任务开始

1. 读取平台入口：`AGENTS.md` 或 `CLAUDE.md`。
2. 读取 `docs/agentic-dev/INDEX.md` 和 `ACTIVE.md`。
3. 检查 `prd-gate.md`、`spec-system.md`、`graphify.md` 和 `skill-manifest.md`。
4. 判断用户请求属于三场景中的哪一类。
5. 根据复杂度决定是否需要 PRD、Spec、Task Queue Harness 或多代理；`/goal` 只在用户显式要求时作为兼容层。

## 三种运行链路

### 小 bug / 小迭代

```text
目的确认
-> systematic-debugging
-> 必要时 Graphify
-> 最小修改
-> verification-before-completion
-> 必要 memory 写回
```

### 单模块开发

```text
Graphify 查询边界
-> prd-test-writer 深聊模块目标和测试基准
-> OpenSpec 变更规范
-> TASKS.md / 单任务文件 / verification-matrix
-> scoped subagents
-> 独立 verifier
-> memory/modules 写回
```

### 全新项目

```text
平台选择
-> bootstrap
-> prd-test-writer 深聊产品和验收
-> Spec-Kit
-> 基础代码骨架
-> Graphify
-> TASKS.md / task-runner / handoff
-> UI/UX / 实现 / 多代理
-> 验证
-> ACTIVE / prd-gate / memory 写回
```

## 写回规则

- 当前任务状态写 `ACTIVE.md`。
- PRD 门禁状态写 `prd-gate.md`。
- 规范系统状态写 `spec-system.md`。
- 工具安装状态写 `skill-manifest.md`。
- 长期事实写 `memory/`。
- medium / large / greenfield 的长跑状态写 `TASKS.md`、`task-runner.md`、`handoff.md` 和 `verification-matrix.md`。

## 完成定义

完成不是“做完计划”，而是“用户目的被可验证地达成”。未验证时只能说明待验证项，不能声称完成。

只要任务队列中仍有 `ready` 或 `verify_failed` task，就不得声称整体完成。`smoke-pass` 只能证明当前 task 通过，不能代表项目结束。
