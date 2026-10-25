---
name: speckit-bugfix
description: >
  处理软件开发阶段测试发现的问题：绑定当前项目，整理并登记 Issue，
  评估问题属于实现、Plan、SPEC 还是新目标，在用户确认处理路径后执行
  缺陷修复与测试，并在用户确认后关闭问题。用户提交测试问题、验收失败、
  回归缺陷，或要求按 Bugfix 小循环修复时使用；不用于尚未进入测试阶段的
  普通新功能开发。
---

# Spec Kit Bugfix

把测试问题从一段口头描述，转化为可追踪的问题记录、范围判断、修复和验证证据。
本 Skill 负责 Bugfix 小循环，也负责判断问题是否必须返回 SDD 主循环。

## 核心约束

1. 绑定调用时的当前项目。先用 Git 根目录或当前工作目录确定项目边界，不要去
   其他仓库寻找同名文件。
2. 先记录，再评估。没有完成 `Issue` 和 `Assess`，不得直接修改代码。
3. `Assess` 是主循环与小循环的分叉点。Agent 可以提出建议，但处理路径必须由
   用户确认。
4. 只有确认属于实现缺陷，才执行 `Fix -> Test`。
5. 自动化测试通过只能标记为 `Fixed`。原始失败场景真实复测通过以后，才能建议
   `Verified`。
6. `Closed` 必须由用户明确确认。不得自行关闭问题。
7. 不得把未执行的测试写成通过，也不得用自动化测试代替真实场景验证。
8. 不得为了让测试通过而放宽已有契约、删除校验或扩大修复范围，除非用户确认
   返回 SPEC 或 Plan。

## Issue 与 Product Backlog 分流

测试、验收或讨论中可能同时出现缺陷和新需求。必须先分流，不能把两者混成一个
Issue：

- **Issue**：当前 SPEC、Plan 或已经确认的验收要求没有被实现，或现有行为违反
  已确认契约。进入问题总表和 `Assess`。
- **Product Backlog**：当前没有承诺、但用户希望新增的能力、规则或产品方向。
  进入项目根目录约定的 `product-backlog.md`，初始状态通常为 `Inbox`。
- **需求澄清**：用户只是提出疑问或讨论方向，尚未表达要新增能力时，不要擅自
  写入 Backlog；可以在当前 Issue 的 Assess 或讨论记录中保留待确认事项。

同一轮反馈可以同时产生 Issue 和 Backlog 条目，但必须分别记录并互相引用。比如
“当前页面没有保留下载地址”可以是 Issue；“增加资源链接索引和混合搜索”则是
新的 Backlog 候选。Backlog 条目不是当前版本承诺，不得直接进入 `tasks.md`、
修改代码或替代 SPEC。

记录 Backlog 前：

1. 先在项目根目录查找已有的 `product-backlog.md` 或项目明确指定的产品待办文件；
2. 沿用项目已有编号、状态和章节结构；没有编号时才建立 `PB-001` 形式的编号；
3. 使用 [references/artifacts.md](references/artifacts.md) 中的最低字段；
4. 只有用户确认条目进入 `Ready for Spec` 后，才通过 `$speckit-specify` 建立
   新需求；不能把候选描述直接当作 SPEC。

## 快速入口

用户可以直接提交原始问题，不需要先整理格式。例如：

```text
使用 $speckit-bugfix 处理这个测试问题：
提交公众号文章时，Agent 生成了后端不接受的资料类型，任务转入人工审核。
```

收到一个或多个问题后，读取 [references/workflow.md](references/workflow.md)
并按其中的阶段执行。创建或更新记录时，读取
[references/artifacts.md](references/artifacts.md)。如果讨论中出现当前 SPEC
之外的新需求或产品能力，还要按其中的 Product Backlog 规范记录。

## 两个人工门禁

### 路径确认

完成只读分析和 `assessment.md` 后暂停，向用户报告：

- 问题编号和复现结果；
- 根因及证据；
- 建议分类：实现缺陷、Plan 调整、SPEC 调整或新目标；
- 建议进入小循环还是从主循环的哪一层继续；
- 获批后会修改的范围。

必须得到用户明确确认，才能修改代码、测试、SPEC 或 Plan。

### 关闭确认

修复与验证完成后再次暂停，报告：

- 实际修改；
- 已运行和未运行的测试；
- 原始场景是否复测；
- 当前建议状态；
- 仍然存在的风险或阻塞。

只有用户明确同意关闭，才能把状态改为 `Closed`。用户未确认时，保留
`Fixed` 或 `Verified`。

## 路径规则

- 新问题的详细记录默认放在
  `specs/<feature>/bugs/<issue-id>-<slug>/`。
- 问题总表优先使用 `specs/<feature>/acceptance-issues.md`。
- `.specify/` 用于 Spec Kit 的脚本、模板、配置和 memory，不作为新 Bug
  记录的默认目录。
- 如果项目已经在其他位置保存该问题，继续使用现有记录，不要制造第二份。
- 教材 Profile、发布资料目录或其他映射视图不是项目执行路径，除非项目明确
  规定它们是源文件。

## 与 Spec Kit 的关系

- 实现缺陷：由本 Skill 完成小循环。
- Plan 调整：用户确认后，从 Plan 层继续，并按项目已有的 Spec Kit 工具执行。
- SPEC 调整：用户确认后，从 SPEC 层继续。
- 新目标：用户确认后，建立 Proposal，再进入完整主循环。

不要用 `$speckit-implement` 直接替代本 Skill。它依赖 `tasks.md`，不会自动完成
Issue 登记、问题分类、人工路径确认和关闭门禁。
