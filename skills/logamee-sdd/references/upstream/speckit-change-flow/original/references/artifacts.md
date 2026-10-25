# Change Flow Artifacts

记录以事实为主。不要把计划写成已经完成，也不要把推测写成根因。

## Issue 总表条目

沿用项目已有的表格或段落结构。没有现成格式时，使用：

```markdown
## AI-003：简短的问题标题

| 字段 | 内容 |
| --- | --- |
| 编号 | AI-003 |
| 发现日期 | YYYY-MM-DD |
| 状态 | Open |
| 类型 | 待评估 |
| 严重程度 | High / Medium / Low |
| 发现环境 | development / acceptance / production |
| 关联 Backlog | 无 / PB-002 |
| Bugfix 记录 | `specs/<feature>/bugs/ai-003-short-slug/` |

### 问题描述

### 复现步骤

### 期望结果

### 实际结果

### 验收条件
```

混合版本接纳后，在 Issue 中增加版本和回写记录，不删除原始复现证据。

## assessment.md

```markdown
# AI-003 Assessment: 问题标题

**Issue**: `AI-003`
**Assessed**: YYYY-MM-DD
**Classification**: 实现缺陷 / Plan 调整 / SPEC 调整 / 新目标 / 混合版本候选
**Mode**: BugFix / New Requirement / Mixed Release

## Original report

## Reproduction

## Expected result

## Findings

## Root cause

## Scope decision

- Issue：保留 / 新建
- Product Backlog：不需要 / PB-002
- SPEC：修改 / 不修改，以及理由
- Plan：修改 / 不修改，以及理由
- Proposal：不需要 / V1.2
- 建议路径：

## Required verification
```

`assessment.md` 只记录判断，不记录尚未执行的修复结果。

## 单独 BugFix 的记录

```text
specs/<feature>/bugs/<issue-id-lower>-<short-slug>/
├── assessment.md
├── fix.md
└── test.md
```

### fix.md

```markdown
# AI-003 Fix

**Status**: Implemented / Blocked
**Implemented**: YYYY-MM-DD

## Failing test

## Changes

## Scope

## Not changed

## Risks
```

### test.md

```markdown
# AI-003 Test

**Status**: Automated tests passed; live scenario pending
**Tested**: YYYY-MM-DD

## Targeted verification

## Regression verification

## Original scenario

## Not run

## Result
```

每条测试写明实际命令、结果和限制。无法运行的测试放在 `Not run`。

## Product Backlog 条目

默认位置是项目根目录的 `product-backlog.md`；如果项目已经指定其他路径，沿用项目
约定，不另建第二份：

```markdown
### PB-002：简短的能力名称

| 字段 | 内容 |
| --- | --- |
| 编号 | PB-002 |
| 名称 | 能力名称 |
| 状态 | Inbox |
| 类型 | Future Capability / Search Capability / Product Improvement |
| 目标版本 | 未确定 |
| 来源 | 测试、用户讨论或需求文档 |
| 触发 Issue | 无 / AI-003 |
| 关联需求 | `specs/<feature>/spec.md` 或无 |

**要解决的问题**

说明用户现在遇到的长期问题，以及为什么现有 SPEC 不足以解决。

**可能包含的能力**

- 候选能力一；
- 候选能力二。

这些内容只是候选方向，不构成当前版本的功能承诺。

**进入 SPEC 的条件**

1. 价值和用户场景已经确认；
2. 范围和边界可以描述；
3. 验收方向可以形成；
4. 用户确认进入 `Ready for Spec`。
```

Product Backlog 状态：

| 状态 | 含义 |
| --- | --- |
| Inbox | 刚记录，尚未整理 |
| Candidate | 值得继续研究，范围尚未确认 |
| Deferred | 明确不进入当前版本 |
| Ready for Spec | 可以建立正式 SPEC |
| Accepted in V1.x | 已被某个版本 Proposal 接纳 |
| Done | 已按版本验收完成 |
| Rejected | 已决定不做，并保留理由 |

## 混合版本的绑定关系

混合版本至少需要在 Proposal 中写清楚：

```markdown
## 本次纳入

- AI-008：原始 Bug 和对应验收场景；
- PB-002：新增产品目标和范围。

## 本次不纳入

- 其他未确认的 Issue；
- 其他仍处于 Inbox 或 Candidate 的 PB。
```

版本测试完成后分别回写：

```text
Issue：验收条件通过，记录测试证据并由 Agent 关闭；若涉及主观判断则等待用户反馈
PB：已由 V1.2 接纳并交付，按客观验收证据更新为 Done
```

## 状态更新

状态变化追加到原记录的变更记录，不覆盖历史：

```markdown
| YYYY-MM-DD | AI-003 | V1.2 版本测试通过，状态改为 Verified |
```

由 Agent 根据客观验收证据关闭时，记录测试依据；需要用户判断的事项则记录用户反馈：

```markdown
| YYYY-MM-DD | AI-003 | 自动化与契约验收通过，Agent 关闭 |
| YYYY-MM-DD | AI-005 | 用户确认页面保留了教程的关键操作步骤，关闭 |
```
