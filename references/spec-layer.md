# 规范层

## 二选一规则

同一个任务只能有一个规范系统事实源：

```text
场景一：原有项目迭代 / bug 修复 -> OpenSpec 按需
场景二：当前项目单模块开发 -> Graphify + OpenSpec 优先
场景三：全新项目开发 -> Spec-Kit 优先
tiny / small -> 通常不用规范系统
```

`prd-test-writer` 不属于规范系统，它是上游需求和测试基准层。PRD 可以喂给 OpenSpec 或 Spec-Kit，但不能替代它们。

## 启用门禁

- medium / large / greenfield 必须先完成深聊对齐。
- 没有 PRD、用户故事、验收标准或测试基准，不允许进入 Spec-Kit implement 或 OpenSpec apply。
- 如果需求仍模糊，先回到 `deep-alignment.md`，不要创建规范文件掩盖不确定性。

## OpenSpec

适合：

- 场景一的中型行为变更。
- 场景二的单模块开发。
- 存量项目中涉及接口、数据、权限、兼容性或边界的改动。

不适合：

- 小 bug、小文案、小配置。
- 全新项目默认启动。

初始化：

```bash
npm install -g @fission-ai/openspec@latest
openspec --version
openspec init --tools codex
openspec init --tools claude
openspec init --tools codex,claude
```

常见流程：

```text
需求不清：/opsx:explore -> /opsx:new -> /opsx:continue -> /opsx:apply
清晰变更：/opsx:new -> /opsx:ff -> /opsx:apply -> /opsx:verify -> /opsx:archive
```

状态写入：

```text
docs/agentic-dev/spec-system.md
openspec/
docs/agentic-dev/prd-gate.md
```

## Spec-Kit

适合：

- 场景三全新项目。
- 从产品目标、用户故事、技术计划到任务拆分的阶段门控。

初始化：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
specify version
specify init . --integration codex --integration-options="--skills"
specify init . --integration claude
```

常见流程：

```text
prd-test-writer 深聊对齐
-> speckit-constitution
-> speckit-specify
-> speckit-clarify
-> speckit-plan
-> speckit-tasks
-> speckit-analyze
-> speckit-implement
```

Codex 的 Spec-Kit skills 模式使用 `$speckit-*`；多数 slash command 平台使用 `/speckit.*`。以实际平台和 CLI 输出为准。

## `spec-system.md` 模板

```md
# 规范系统

## 当前选择
- 规范系统：OpenSpec / Spec-Kit / none
- 选择原因：
- 适用场景：
- 初始化状态：installed / pending / skipped / blocked / failed

## 上游事实源
- PRD：
- 测试基准：
- `prd-gate.md` 状态：

## 入口
- 命令：
- 目录：
- skills / commands 路径：

## 禁止事项
- 同一任务不得同时使用 OpenSpec 和 Spec-Kit。
- 未完成深聊对齐不得进入 apply / implement。
- 初始化失败不得声称已完成规范层。
```

## 混用处理

如果发现同一任务同时出现 OpenSpec 与 Spec-Kit：

1. 停止实现。
2. 选择唯一事实源。
3. 另一套只归档为参考或删除未使用产物。
4. 更新 `spec-system.md`、`ACTIVE.md` 和最终回复。
