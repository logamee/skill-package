---
name: speckit-change-flow
description: >
  管理 Spec Kit 项目在开发和测试阶段发现的变更：把反馈分流为单独 BugFix、
  单独新增需求，或 Bug 与新增需求组成的混合版本，并在必要的人工确认后驱动对应的
  Issue、Product Backlog、Proposal、SPEC、Plan、Tasks、实现和验证流程。
  用户提交测试问题、验收失败、回归缺陷、新需求，或要求判断走小循环还是版本
  主循环时使用；不用于普通代码修改或尚未确认的随想讨论。
---

# Spec Kit Change Flow

这个 Skill 是 Spec Kit 的变更分流层。它不把所有反馈都当成 Bug，也不直接替用户
决定进入小循环还是主循环。

核心分流只有三种模式：

```text
测试反馈 / 用户变更请求
        ↓
      Assess
        ├─ 只有实现缺陷       → BugFix 小循环
        ├─ 只有新增产品目标   → Product Backlog → 主循环
        └─ Bug + 新增产品目标 → Issue + Product Backlog → 版本主循环
```

读取以下参考资料：

- 具体阶段和人工门禁：`references/workflow.md`
- Issue、Backlog、版本绑定和测试记录格式：`references/artifacts.md`

## 基本原则

1. 先绑定当前项目，再读取当前项目的 SPEC、Plan、Tasks、测试和记录。
2. 先记录事实，再判断路径；没有完成 Assess，不得直接修改代码。
3. `Assess` 是分叉点。Agent 可以提出建议，但路径、范围和版本归属必须由用户确认。
4. 已有契约未被实现，是 BugFix；新增产品目标，才进入 Product Backlog。
5. 修正已有目标的 SPEC 或 Plan，不自动伪装成新增需求；只有产生新的用户目标时才新增 PB。
6. 混合版本中，Issue 和 Product Backlog 都必须保留，通过版本 Proposal 绑定，不得把
   Bug 悄悄改写成普通需求。
7. Agent 负责执行所有可自动化或可客观核验的测试。验收条件已被这些测试覆盖且通过时，Agent 应自行标记 `Verified` 并关闭 Issue，不要求用户重复测试。
8. 所有验收只分为两类：Agent 收尾，或用户人工收尾。UI、视觉效果、Markdown/HTML 的语义呈现、文章结构、文案质量、信息提炼和交互体验，即使可以做技术检查，也不能仅凭技术检查关闭，必须交由用户人工验收。
9. 只有涉及确定规则、可重复计算、明确枚举、严格格式、状态转换、数据完整性或可定义容差的结果，Agent 才可以自行验收并关闭。例如：计算结果在规定范围内、字段符合 Schema、状态按规则流转、资源引用确实存在。
10. 需要人工验收时，Agent 必须说明用户要看什么、通过标准是什么，以及哪些部分已经由 Agent 验证而无需重复检查。
11. 不得为了让测试通过而放宽已有校验、删除契约或扩大修复范围。

## 模式一：单独修复 Bug

适用于 SPEC 和 Plan 已经正确，但实现没有满足已有要求：

```text
Issue → Assess → 用户确认
      → 失败测试 → Fix → Test
      → Fixed → 客观验收通过后由 Agent 标记 Verified / Closed
```

此模式不建立 Product Backlog，不编写 Proposal、需求简报、SPEC 或 Plan。

## 模式二：单独新增需求

适用于当前 SPEC 没有承诺，但用户希望增加新的能力、规则或产品目标：

```text
Product Backlog
        ↓
用户确认进入 Ready for Spec
        ↓
SPEC → Plan → Tasks → Implement → Test
```

Product Backlog 是需求入口和来源记录，不是需求简报。新需求已经清楚时，不要为了
形式再写一份需求简报。

如果项目按版本交付，可以在 SPEC 前建立或更新版本 Proposal；Proposal 只负责确定
版本是否接纳这个需求及范围，不重复展开实现细节。

## 模式三：Bug 与新增需求组成混合版本

适用于一次变更同时包含：

- 一个当前契约没有被实现的 Bug；
- 一个当前 SPEC 没有承诺的新产品目标。

必须分别登记：

```text
Issue：记录 Bug 的事实、复现过程和失败证据
Product Backlog：记录新增需求的来源、目标和候选范围
```

然后由版本 Proposal 统一接纳：

```text
AI-008 ─┐
        ├─→ V1.2 Proposal → SPEC → Plan → Tasks → Implement → Test
PB-002 ─┘
```

一旦用户确认采用混合版本，就不再对 Bug 单独执行 `Fix → Test` 小循环。Bug 的修复
要求应转化为版本 SPEC 中的验收场景，并与新增需求共用同一份 Plan、Tasks 和版本测试。
原 Issue 和 PB 仍然保留，版本验收完成后分别回写结果。

## Product Backlog 的使用边界

必须写入 Product Backlog：

- 当前 SPEC 没有承诺的新能力；
- 用户新增的产品规则或搜索方式；
- Bug 修复过程中提出的额外能力；
- 决定延后到未来版本的目标。

不应仅因为它不是代码 Bug 就写入 Product Backlog：

- 已有 SPEC 正确、但代码实现错误；
- 已有目标不变，只需修正 Plan；
- 需要修正文字、验收条件或实现方案，但没有新的用户目标。

如果同一轮反馈同时包含两类内容，分别记录 Issue 和 PB，并在两边互相引用。

## 人工门禁

### 路径确认

完成只读分析和记录后，必须向用户报告：

- 问题或需求编号；
- 复现结果或需求来源；
- 根因和证据；
- 建议模式：BugFix、单独新增需求或混合版本；
- 是否涉及 SPEC、Plan 或 Proposal；
- 获批后会修改的范围。

只有用户确认后，才能修改代码、SPEC、Plan、Tasks 或版本资料。

### 验收责任与关闭

修复或版本测试完成后，必须报告：

- 实际修改；
- 已运行和未运行的测试；
- 原始场景是否复测；
- Issue 和 PB 当前状态；
- 剩余风险。

先判断由谁收尾：

| 验收类型 | 例子 | 负责人 |
| --- | --- | --- |
| Agent 收尾 | 数值计算、范围、枚举、Schema、状态流转、数据完整性、链接存在性等，输入、规则和预期结果可以明确写出 | Agent |
| 用户收尾 | UI、视觉效果、Markdown/HTML 最终呈现、标题层级、重点突出、文章结构、内容提炼、交互体验和用户偏好 | 用户 |

用户收尾的问题，Agent 可以先做技术预检，但技术预检不等于验收。例如：

- Agent 可以确认 Markdown 的 `#` 被转换成标题节点；
- Agent 不能仅凭节点存在就判断标题层级是否合理，因此整个问题仍由用户收尾；
- Agent 可以确认按钮可点击；
- Agent 不能仅凭点击成功就判断 UI 布局、文案和交互体验合格，因此整个问题仍由用户收尾。

属于 Agent 收尾的问题，由自动化测试、契约检查或真实环境读回充分覆盖后，Agent
自行更新为 `Verified` 并关闭。属于用户收尾的问题，Agent 必须说明用户要检查什么、
通过标准是什么，并保持未关闭；用户不需要重复执行已经完成的技术预检。

## 与 Spec Kit Skills 的关系

- 单独 BugFix：本 Skill 完成记录、Assess、Fix 和 Test。
- 单独新增需求：本 Skill 负责 PB 和路径确认，然后调用 `$speckit-specify`、
  `$speckit-plan`、`$speckit-tasks` 和 `$speckit-implement`。
- 混合版本：本 Skill 负责 Issue、PB、版本 Proposal 和路径确认，然后进入版本级的
  SPEC、Plan、Tasks 和实现。
- Plan 或 SPEC 调整：保留原 Issue 和 Assess，不把它们伪装成实现缺陷。

不要直接使用 `$speckit-implement` 替代本 Skill；它不会自动完成变更分流、Issue
登记、Product Backlog 记录和人工门禁。
