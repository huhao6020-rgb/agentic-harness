# 验证手册

## 结构验证

- `SKILL.md` frontmatter 只写触发条件，不塞完整流程。
- `SKILL.md` 能路由到 `bootstrap-phases.md`、`deep-alignment.md`、`bootstrap-contract.md`、`install-skills.md`、`spec-layer.md`。
- references 包含 `bootstrap-phases.md`。
- references 包含 `deep-alignment.md`。
- 所有项目模板标题和说明使用简体中文。
- 不出现旧版 skill 名称。
- 不出现自定义 `.codex/agentic-dev`、`.codex/goals`、`.codex/tasks`。

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

不得把 `agent-reach` 写成项目级依赖。

## 压力场景

| Prompt | 期望 |
| --- | --- |
| 使用 agentic-harness，新建自由画布应用 | 先问平台，再 bootstrap，再 PRD，再 Spec-Kit |
| 使用 agentic-harness，构建当前项目自由画布模块 | Graphify + PRD 门禁 + OpenSpec |
| 优化自由画布拖拽卡顿 | debugging，必要时 Graphify，不启 Spec-Kit/OpenSpec |
| 我也说不清，就是感觉不好用 | 进入深聊，不写代码 |
| 继续昨晚的任务 | 读取 `ACTIVE.md`、`prd-gate.md`、spec 状态和验证矩阵 |
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
- 小 bug：不得生成 PRD、Spec、goal、多代理和大量文档。

## 完成验证

声称完成前必须至少提供一类证据：

- 测试通过。
- 构建通过。
- lint / typecheck 通过。
- 浏览器或等价 UI 验收通过。
- OpenSpec verify 通过。
- Spec-Kit analyze / checklist / implement 后的验证通过。
- 无法验证时明确说明原因和残余风险。
