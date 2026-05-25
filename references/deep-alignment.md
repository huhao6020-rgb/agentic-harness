# 深聊对齐协议

## 目的

深聊对齐是 harness 的第一道门禁。它解决的不是“写什么文档”，而是避免 Agent 带着半懂不懂的需求开始长跑、搭架构或并行开发。

## 任务重量与对齐方式

| 任务重量 | 对齐要求 | 允许继续的条件 |
| --- | --- | --- |
| tiny | 一句话目标 + 最小验收点 | 用户目标清楚，影响范围极小 |
| small | 目标、范围、非范围、验收点 | 可以直接验证，不影响架构边界 |
| medium | 使用 `prd-test-writer` 或等价流程 | PRD、用户故事、验收标准、测试基准明确 |
| large | `prd-test-writer` + plan/tasks + verification matrix | 可恢复、可分工、可验证 |
| greenfield | `prd-test-writer` 先于 Spec-Kit | 产品目标、用户、场景、成功标准、非目标明确 |

## 何时必须使用 `prd-test-writer`

- 用户说“优化”“构建”“重做”“不好用”“做一个产品/应用”，但目标或验收不清。
- 涉及产品行为、用户故事、页面流程、交互体验、业务规则或测试基准。
- 任务准备进入 OpenSpec、Spec-Kit、官方 `/goal`、多代理或长时间无人值守执行。
- 需要把需求转化为可执行测试用例或验收矩阵。

`prd-test-writer` 的产物是上游事实源：

```text
PRD -> Spec-Kit / OpenSpec
测试用例 -> verification matrix
用户故事 -> tasks
验收标准 -> Done Means
Review 问题 -> 下一轮 PRD / Spec 修订
```

## 产物位置

默认写入：

```text
docs/prd/
docs/PRD_REGISTRY.md
docs/agentic-dev/prd-gate.md
```

`prd-gate.md` 至少记录：

```md
# PRD 门禁

## 当前任务
- 任务名称：
- 任务重量：tiny / small / medium / large / greenfield
- 当前状态：not-needed / pending / aligned / blocked

## 对齐结论
- 用户目标：
- 成功标准：
- In Scope：
- Out of Scope：
- 验收信号：
- 测试基准：

## 后续入口
- PRD：
- 测试用例：
- 规范系统：
- 下一步：
```

## 停止条件

遇到以下情况不得进入实现：

- 用户目标只能描述为“感觉不好”“优化一下”“做得高级点”，但没有可验证结果。
- medium / large / greenfield 没有 PRD 或等价对齐记录。
- 要启动 `/goal`、多代理、Spec-Kit implement 或 OpenSpec apply，但没有验收标准。
- `prd-test-writer` 缺失且用户未同意降级为手动对齐。

## 降级方式

如果 `prd-test-writer` 不存在：

1. 不创建空 skill，不伪造产物。
2. 在 `docs/agentic-dev/skill-manifest.md` 标记 `prd-test-writer = pending`。
3. 问用户是否安装或允许手动深聊对齐。
4. 用户允许降级时，按 tiny/small/medium 的字段手动完成 `prd-gate.md`。
