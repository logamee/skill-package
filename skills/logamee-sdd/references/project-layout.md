# Project Documentation Layout

先沿用项目现有结构。新项目可以从下面的 SDD 目录开始；源码和测试仍放在实际工程目录，
不放进 SDD 文档目录。所有示例路径都以项目根目录的 `sdd/` 为起点。

```text
project/
├── sdd/
│   ├── constitution.md              # 有项目原则时建立
│   ├── project.md                   # 正式 SDD 项目说明
│   ├── product-backlog.md           # 有未承诺目标时建立
│   ├── releases.md                  # 正式版本基线登记
│   ├── bootstrap/                    # V1 基线形成前的轻量开发记录
│   │   ├── bootstrap-spec.md         # Bootstrap 阶段的轻量需求与行为规格
│   │   ├── plan.md                   # 轻量技术实现计划
│   │   └── issues.md                 # Bootstrap 问题台账
│   ├── specs/
│   │   └── 001-feature-name/
│   │       ├── brief.md
│   │       ├── spec.md
│   │       ├── plan.md
│   │       ├── tasks.md
│   │       ├── acceptance.md
│   │       └── implementation-report.md
│   └── changes/
│       ├── CHG-001-change-name/
│       │   ├── target.md
│       │   ├── proposal.md
│       │   ├── change-spec.md
│       │   ├── tasks.md              # 复杂变更时建立
│       │   ├── acceptance.md
│       │   └── implementation-report.md
│       └── archive/
├── apps/ | src/ | workers/ | packages/
└── tests/
```

一个 Spec 对应稳定的业务能力，不为每个小功能新建 Spec。一个 Change 可以影响多个 Spec；
按职责分别更新这些 Spec，并由同一个 Proposal / Plan / 验收范围关联。

Change 进行期间，相关 Spec 目录中的完整 `spec.md` 和 `plan.md` 是后续实现的工作依据。
没有项目级暂存约定时，直接在原文件上形成目标工作版本，并在 Change 中标记
`In Progress`；正式基线由 `releases.md` 和项目版本记录确认。Change 被废弃时，若项目
有版本控制则从变更前的基线恢复；没有版本控制时先保留变更前资料，再进行恢复。不要
创建脱离主 Plan 的独立增量 Plan，也不要让实现只依赖 `change-spec.md`。

正式 SDD 项目至少建立 `sdd/project.md` 和 `sdd/releases.md`；`constitution.md`、
`product-backlog.md` 按实际需要建立。Spec 编号使用三位数字，从现有最大编号递增；
名称使用简短的 kebab-case，例如 `001-news-ingestion`。问题编号和版本号沿用项目已有
约定；只有项目尚无约定时，才采用本 Skill 示例中的默认格式。Bootstrap 项目只需要
`sdd/bootstrap/bootstrap-spec.md`、`sdd/bootstrap/plan.md` 和 `sdd/bootstrap/issues.md`，不因
初始化而创建正式 SDD 根文档。
