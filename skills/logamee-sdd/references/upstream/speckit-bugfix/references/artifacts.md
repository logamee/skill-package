# Bugfix Artifacts

记录以事实为主。不要把计划写成已经完成，也不要把推测写成根因。

## 问题总表条目

沿用项目现有表格或段落结构。没有现成格式时，使用：

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
| Bugfix 记录 | `specs/<feature>/bugs/ai-003-short-slug/` |

### 问题描述

### 复现步骤

### 期望结果

### 实际结果

### 验收条件
```

## assessment.md

```markdown
# AI-003 Assessment: 问题标题

**Issue**: `AI-003`
**Assessed**: YYYY-MM-DD
**Classification**: 实现缺陷 / Plan 调整 / SPEC 调整 / 新目标

## Original report

## Reproduction

## Expected result

## Findings

## Root cause

## Scope decision

- SPEC：修改 / 不修改，以及理由
- Plan：修改 / 不修改，以及理由
- 实现：预计影响范围
- 建议路径：Bugfix / Plan / SPEC / Proposal

## Required verification
```

`assessment.md` 记录判断，不记录尚未执行的修复结果。

## fix.md

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

实际修改与 `assessment.md` 不一致时，暂停并重新确认范围，不能事后把扩大范围
悄悄写进 `fix.md`。

## test.md

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

每条测试写明实际命令、结果和约束。无法运行的测试放在 `Not run`，并说明原因。

## 状态更新

状态变化追加到问题总表的变更记录，不覆盖历史：

```markdown
| YYYY-MM-DD | AI-003 | 完成最小修复和自动化测试，状态改为 Fixed |
```

关闭时记录用户确认：

```markdown
| YYYY-MM-DD | AI-003 | 原始场景验证通过，用户确认关闭 |
```

## Product Backlog 条目

当讨论中产生当前 SPEC 没有承诺的新能力、产品方向或延期事项时，使用项目已有的
Product Backlog 文件记录。默认位置是项目根目录的 `product-backlog.md`；如果项目
已经指定其他路径，沿用项目约定，不另建第二份 Backlog。

Backlog 条目至少包含以下字段：

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
| 关联需求 | `specs/<feature>/spec.md` 或无 |

**要解决的问题**

说明用户现在遇到的长期问题，以及为什么现有 SPEC 不足以解决。

**可能包含的能力**

- 候选能力一；
- 候选能力二；
- 候选能力三。

这些内容只是候选方向，不构成当前版本的功能承诺。

**暂缓原因**

说明为什么暂时不进入当前 SPEC 或实现。

**进入 SPEC 的条件**

1. 价值和用户场景已经确认；
2. 范围和边界可以描述；
3. 验收方向可以形成；
4. 用户确认进入 `Ready for Spec`。

**后续动作**

满足条件后，为该条目单独执行 `$speckit-specify`，再进入 Plan、Tasks 和
Implement 流程。
```

### Product Backlog 状态

| 状态 | 含义 |
| --- | --- |
| Inbox | 刚记录的想法，尚未整理和判断 |
| Candidate | 值得继续研究，但范围和价值尚未确认 |
| Deferred | 已讨论，明确不进入当前版本 |
| Ready for Spec | 范围和验收方向足够清楚，可以建立独立 SPEC |
| Rejected | 已决定不做，并保留理由 |

不要因为一个测试问题自然延伸出新想法，就把新想法直接写入 Issue 的修复范围。
应在 Issue 中记录关联的 Backlog 编号，在 Backlog 中记录触发它的 Issue 编号。
