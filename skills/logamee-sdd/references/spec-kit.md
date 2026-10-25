# Spec Kit Stage Protocol

本模块将 GitHub Spec Kit 的核心阶段映射到 `logamee-sdd`。上游命令模板快照位于
`upstream/spec-kit/commands/`；它们是方法来源，不要求当前环境安装独立 Spec Kit Skill。

## 阶段映射

| 阶段 | 输入 | 处理 | 输出 |
| --- | --- | --- | --- |
| Specify | `brief.md`、项目宪章、项目背景 | 把目标转成用户场景、可验证需求和边界；在同一份文档内澄清修订 | 定稿 `spec.md` |
| Plan | 完整目标 `spec.md`、项目约束、源码结构 | 研究未决技术问题，形成架构、数据与接口设计，并检查需求覆盖 | 完整 `plan.md`，必要时附研究、数据模型、契约和验证指南 |
| Tasks | 定稿 `spec.md`、`plan.md`、现有代码与测试 | 按依赖拆成可执行任务，关联需求或用户故事 | 按复杂度生成 `tasks.md`；小改动可跳过 |
| Implement | 完整目标 `spec.md`、完整目标 `plan.md`、Tasks（如有） | 按任务实现、运行针对性测试和适当回归，记录未运行项目 | 源码、测试、实施报告与验收记录 |

## 本项目的覆盖规则

- 顺序沿用 `specify → plan → tasks（按需）→ implement`，不要求 `.specify/` 默认目录。
- 有修改需求时，阅读完整原 SPEC 和完整原 Plan；Change Spec 不能单独作为实现依据。
- 同一 SPEC 从草稿修订为定稿，不重复创建“草稿 Spec”和“审核 Spec”。
- Change 先形成增量 Change Spec，再将其应用到原完整 SPEC，得到完整目标 SPEC；Plan 同样在原完整 Plan 上更新。
- 质量检查、讨论和用户确认嵌入相应阶段，不再增加独立人工审核阶段。
- 上游命令中的钩子、脚本、自动分支、批量澄清问题等只有在项目明确安装且本 Skill 规则允许时才使用。
- 若上游模板与本 Skill 对文档位置、逐题访谈、目标 SPEC/Plan 或验收责任的约定冲突，以本 Skill 为准。
