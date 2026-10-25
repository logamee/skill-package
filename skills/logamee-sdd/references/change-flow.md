# Change Assessment & Flow Protocol

本模块整合本地 `speckit-change-flow` 与 `speckit-bugfix`。原始文件保存在
`upstream/speckit-change-flow/` 和 `upstream/speckit-bugfix/`；本文件是 `logamee-sdd`
实际执行的统一规则，并落实已确认的 Change Spec / 完整目标 Spec / 完整目标 Plan 流程。

Change 编号、Issue 编号和版本号沿用项目已有规范。文中的 `CHG-001`、`AI-001`、
`V1.1` 等均为示例；项目尚无约定时，可以采用这些格式作为默认值。

`Change Flow` 是总流程名称，但进入流程后必须先做 **Spec 关系评估**。不能只判断
“这是 Bug 还是新需求”，还要判断它属于哪个规格、是否需要更新规格，以及是否应该
新建规格。

## 评估顺序

读取相关完整 SPEC、Plan、代码、测试和现有记录，记录事实后按以下顺序判断。

### 1. 判断当前行为是否已经被规格承诺

| 判断 | 结论 |
| --- | --- |
| 当前 SPEC 已经明确要求该行为，但代码没有做到 | 实现 Bug，进入 BugFix 小循环 |
| 当前 SPEC 没有要求，但用户只是希望技术方案换一种实现 | Plan 调整，不改变产品规格 |
| 当前 SPEC 对产品行为的定义不正确或需要改变 | 规格变更，进入 Change 主流程 |
| 当前 SPEC 没有承诺，而用户提出新的用户价值或业务目标 | 新增目标，继续判断新建规格还是归入已有规格 |

### 2. 判断目标属于已有规格还是新规格

| Spec 关系 | 判断标准 | 文档动作 |
| --- | --- | --- |
| 更新已有规格 | 新行为仍然属于某个已有业务能力，只是增加、修改或取消其中的行为 | `target.md` 指向已有 Spec，更新该 Spec |
| 新建规格 | 新行为形成独立、稳定、可单独验收的业务能力，现有 Spec 没有合适的归属 | `target.md` 标记 New Spec，创建新的 Spec |
| 多规格变更 | 一个变更同时影响多个已有业务能力，且各能力的职责边界仍需保留 | `target.md` 列出多个受影响 Spec，分别更新 |
| 新旧规格并存 | 新能力独立成立，同时需要修改已有能力才能完成集成 | 同一个 Change 同时记录 New Spec 和 Existing Spec |
| 不涉及规格 | 纯实现修复、测试补充、重构或 Plan 技术调整，没有改变用户可见契约 | 不创建或更新 Spec |

不要把每个小功能都新建成 Spec。只要功能仍是已有能力的自然扩展，就更新原 Spec；
只有形成独立业务能力、独立用户价值和独立验收边界时，才新建 Spec。

### 3. 判断是否需要更新 Plan

- BugFix：原 SPEC 和 Plan 都正确，不更新二者。
- Plan 调整：目标和 SPEC 不变，只更新完整 Plan。
- 已有规格变更、新规格或多规格变更：以完整目标 SPEC 为输入，在原完整 Plan 基础上更新。
- 不使用脱离原完整 Plan 的孤立增量 Plan 作为实现依据。

### 4. 选择处理路径

| 最终分类 | 入口 | 是否改变 SPEC |
| --- | --- | --- |
| 实现 Bug | Issue → BugFix → Test | 否 |
| Plan 问题 | Issue / Assess → 更新完整 Plan → Implement → Test | 否 |
| 已有规格变更 | Change → Change Spec → 更新已有完整 SPEC → 更新完整 Plan | 是，更新 |
| 新规格 | Change → Change Spec / New Spec → 创建新 SPEC → Plan | 是，新建 |
| 多规格变更 | Change → Change Spec → 更新或创建多个目标 SPEC → 完整 Plan | 是，多目标 |
| Bug + 规格变更 | Issue + Change → 统一版本 Proposal → 按上述规格路径实现 | 分别保留 Bug 来源和规格变更 |

## 分类总表

| 分类 | 判断依据 | 路径 |
| --- | --- | --- |
| Bug | 已承诺的行为没有实现 | Issue → BugFix → 测试与客观验收 |
| Plan 调整 | 目标不变，原技术方案不成立 | 更新完整 Plan → 实现与测试 |
| 已有 SPEC 调整 | 既有目标或行为定义需要修正，且仍属于已有能力 | Change → 更新已有完整 SPEC → 更新完整 Plan |
| 新 SPEC | 当前没有合适的规格归属，形成独立能力 | Change → 创建新 SPEC → Plan |
| 多 SPEC 变更 | 一个 Change 影响多个规格 | Change → 分别更新或创建目标 SPEC → 完整 Plan |
| 混合变更 | Bug 与新目标决定一起交付 | Issue 与 Backlog 分开记录，由版本 Proposal 绑定 |

一个请求含多个独立问题时分别记录。Issue 记录已承诺但未实现的行为；Product Backlog
记录尚未承诺的新目标，两者不可互相替代。是否进入新版本或改变产品范围时，在当前阶段向用户确认。

Assess 不要求固定创建单独文件。简单情况把分类、证据和建议路径写入 Issue 或
`target.md`；只有分类复杂、需要保留多轮决策时才增加 `assessment.md`。无论采用哪种
记录方式，都必须保留“事实、分类、依据、下一步路径”。

## Change 主流程

1. **记录来源与目标**：为变更建立 `target.md` 和 `proposal.md`。`target.md` 记录 Change
   身份、状态、变更类型、Spec 关系及受影响的 Spec；不写代码、文件或技术模块。
   Proposal 记录目标、范围、非目标和关联 Issue / Backlog。
2. **生成 Change Spec**：读取 Proposal、Target、所有相关完整 SPEC 和项目约束，产出描述本次增删改的
   `change-spec.md`。对于新规格，在其中描述新能力的规格边界；对于已有规格，在其中描述增量。
   在同一份文档中澄清、修订并定稿。
3. **形成完整目标 SPEC**：已有规格将 Change Spec 应用到原完整 SPEC；新规格创建新的完整 SPEC；
   多规格变更按职责分别形成各自的完整目标 SPEC。Change Spec 留作变更历史，完整目标 SPEC
   才是后续工作的依据。没有项目级暂存约定时，直接更新相关规格目录中的完整 `spec.md`，
   在 Change 中标记为工作版本，不把它当作正式版本基线。
4. **更新完整目标 Plan**：以原有完整 Plan 为基础，结合全部完整目标 SPEC、源码结构和必要研究更新；
   直接更新相关规格目录中的完整 `plan.md`；不以孤立的增量 Plan 指导实现。一个 Change
   影响多个 Spec 时，每个 Spec 分别更新自己的 `plan.md`；跨 Spec 的依赖、实施顺序和集成
   关系写入同一个 `proposal.md`，不创建脱离这些 Spec 的统一执行 Plan。
5. **Tasks 与实现**：复杂变更按需生成 Tasks。实现时读取完整目标 SPEC 和 Plan；测试覆盖新增行为、
   原始 Bug 场景及适当回归。
6. **验收与基线**：记录验收证据和实施报告。通过后将完整目标 SPEC / Plan 确认为新基线，
   更新 releases，并归档 Change 资料。

## BugFix 小循环

适用于原 SPEC / Plan 正确而代码实现错误的情形：Issue → Assess → 建立或运行失败测试 →
最小修复 → 针对性测试与回归 → 记录证据。不得借 BugFix 偷加功能、放宽契约或无关重构。
如果发现目标或技术方案本身有误，返回相应的 SPEC 或 Plan 阶段。

BugFix 也要记录它所违反的已有 Spec，但这只是问题来源和验收依据，不代表要修改该
Spec。只有确认原规格本身需要改变时，才从 BugFix 小循环返回 Change 主流程。

## 验收责任

- 可重复、规则明确、可客观判定的结果由 Agent 测试并记录证据；满足条件后可标记 Verified 并关闭。
- UI、视觉、内容取舍、交互体验、偏好或真实业务判断由用户验收；Agent 可先做技术预检，但不能把预检当作通过。
- 每次报告已运行和未运行的测试、原始场景是否复测、结果和剩余风险。
- 人工确认嵌入路径选择、范围决定和主观验收等相应节点，不建立独立的“人工审核”流程步骤。

## Change 归档内容

至少保留 `target.md`、`proposal.md`、`change-spec.md`、Change Tasks（如有）、
`acceptance.md` 与 `implementation-report.md`。可附 `plan-update-summary.md` 作为历史摘要，
但它不能替代完整目标 Plan。

## `target.md` 最小字段

```markdown
# CHG-001：变更名称

| 字段 | 内容 |
| --- | --- |
| 状态 | Proposed |
| 变更类型 | 已有 SPEC 调整 / 新 SPEC / 多 SPEC 变更 / Bug + 规格变更 |
| 受影响的已有 SPEC | `sdd/specs/001-xxx/spec.md` / 无 |
| 需要新建的 SPEC | `sdd/specs/002-yyy/spec.md` / 无 |
| 是否涉及 Bug | 无 / AI-001 |
| 目标版本 | V1.1 |
```

`target.md` 只回答“这次 Change 改哪个规格、是否新建规格、是否包含 Bug”；具体业务行为
放在 `proposal.md` 和 `change-spec.md`，技术方案放在完整目标 `plan.md`。

路径字段统一使用 `sdd/specs/...`；上表中的已有规格示例应理解为：
`sdd/specs/001-xxx/spec.md`。
