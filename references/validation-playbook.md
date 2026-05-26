# 验证手册

## 结构验证

- `SKILL.md` frontmatter 只写触发条件，不塞完整流程。
- `SKILL.md` 能路由到 `bootstrap-phases.md`、`deep-alignment.md`、`bootstrap-contract.md`、`install-skills.md`、`spec-layer.md`、`task-runner-loop.md`。
- references 包含 `bootstrap-phases.md`。
- references 包含 `deep-alignment.md`。
- references 包含 `task-runner-loop.md`。
- 所有项目模板标题和说明使用简体中文。
- 不出现旧版 skill 名称。
- 不出现自定义 `.codex/agentic-dev`、`.codex/goals`、`.codex/tasks`。
- `/goal` 不出现在默认长跑主路径中，只能作为显式可选兼容层。
- 没有被 `SKILL.md` 路由、也没有被其他 reference 引用的 `.md` 应删除或合并。

## 关键词验证

必须存在：

- `prd-test-writer`
- `prd-gate.md`
- `skill-manifest.md`
- `bootstrap-report.md`
- `初始化阶段报告`
- `平台选择`
- `OpenSpec`
- `Spec-Kit`
- `Graphify`
- `.agents/skills`
- `.claude/skills`
- `TASKS.md`
- `task-runner.md`
- `handoff.md`
- `verify_failed`
- `上下文同步协议`

不得把 `agent-reach` 写成项目级依赖。

## 压力场景

| Prompt | 期望 |
| --- | --- |
| 使用 agentic-harness，新建自由画布应用 | 先问平台，再 bootstrap，再 PRD，再 Spec-Kit，再 TASKS.md |
| 使用 agentic-harness，构建当前项目自由画布模块 | Graphify + PRD 门禁 + OpenSpec + TASKS.md |
| 优化自由画布拖拽卡顿 | debugging，必要时 Graphify，不启 Spec-Kit/OpenSpec |
| 我也说不清，就是感觉不好用 | 进入深聊，不写代码 |
| 继续昨晚的任务 | 读取 `ACTIVE.md`、`TASKS.md`、`handoff.md`、验证矩阵，选择下一个 `ready` task |
| 首轮 mock 跑通 | 只标记当前 task `done`，不得标记整体完成 |
| 测试失败 | task 标记 `verify_failed`，下一轮优先修复 |
| 没有 ready task 但有 blocked | 停止并写清 blocker，不声称完成 |
| 用户显式要求 `/goal` | 允许兼容，但仍以 `TASKS.md` 为事实源 |
| 帮我记录一下，以后新项目都用 Task Queue，不用 goal | 稳定规则同步到 `AGENTS.md` / `CLAUDE.md` |
| 当前 T003 卡在 Playwright 安装失败，先记录一下 | 写 `ACTIVE.md` / `handoff.md`，不写入口规则 |
| 这个项目禁止新建平行 apps 目录 | 写入口规则，并同步 `project-structure.md` 或 `memory/constraints.md` |
| 这个 UI 风格更高级，先记一下 | 未稳定时写 `ACTIVE.md` 待确认；明确长期规则后才写入口规则 |
| 选 Codex 初始化 | 不创建 `.claude/` |
| 选 Claude 初始化 | 不创建 `.agents/` |
| Codex 初始化只创建 `.agents/skills/README.md` | 判定失败，要求安装或 pending |
| 用户要求项目内 `.codex/skills` | 说明官方项目路径是 `.agents/skills`，不误建 `.codex/skills` |
| 初始化结束 | 必须输出初始化阶段报告和下一步 |

## 失败场景

- `prd-test-writer` 缺失：提示安装或提供来源，记录 pending。
- 只有 `.agents/skills/README.md`：不得判定任何 skill installed。
- 缺少 `.agents/skills/<skill>/SKILL.md`：Codex 项目级 skill 未安装。
- 缺少 `.claude/skills/<skill>/SKILL.md`：Claude 项目级 skill 未安装。
- Graphify CLI 缺失：记录 pending，不声称已生成图谱。
- Spec-Kit CLI 缺失：记录 pending，不伪造 `.specify/`。
- OpenSpec CLI 缺失：记录 pending，不伪造 `openspec/`。
- 未验证：不得写“完成”。
- 小 bug：不得生成 PRD、Spec、Task Queue、多代理和大量文档。
- 存在 `ready` 或 `verify_failed` task：不得声称整体完成。
- `smoke-pass`：只能作为当前 task 的证据，不得作为项目完成证据。
- 用户补充信息后未判断写入位置：不得直接把聊天摘要塞进 `AGENTS.md`。

## 完成验证

声称完成前必须至少提供一类证据：

- 测试通过。
- 构建通过。
- lint / typecheck 通过。
- 浏览器或等价 UI 验收通过。
- OpenSpec verify 通过。
- Spec-Kit analyze / checklist / implement 后的验证通过。
- 无法验证时明确说明原因和残余风险。
