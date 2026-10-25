<div align="center">

# Logamee Skill Package

**面向 Agent 的 Logamee Skills 合集。**

每个 Skill 都是一个可以独立安装、加载和使用的目录。

</div>

---

## 这是什么

这是 Logamee 的公开 Skill 合集仓库。它不提供一个需要单独启动的应用，
而是提供可被 Agent 读取的 `SKILL.md`、参考资料、评估样例和可选辅助资源。

当前仓库收录：

| Skill | 用途 |
| --- | --- |
| [`logamee-sdd`](skills/logamee-sdd/README.md) | 推进软件项目从需求讨论、SPEC、Plan、实现、测试、验收，到版本变更的 SDD 流程。 |

后续 Skill 会继续放在 `skills/` 下，每个 Skill 保持独立目录和独立说明。

## 快速开始

请把下面这句话复制给你的 Agent：

> 请从 GitHub 仓库 https://github.com/logamee/skill-package 获取并安装 `logamee-sdd` Skill。

也可以手动安装：

```bash
git clone https://github.com/logamee/skill-package.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skill-package/skills/logamee-sdd "${CODEX_HOME:-$HOME/.codex}/skills/"
```

不同 Agent 的 Skill 发现目录可能不同。请把 `skills/logamee-sdd/`
复制或链接到对应 Agent 的 Skill 目录，并保留该目录中的 `SKILL.md`、
`references/`、`agents/` 和 `evals/`。

## 使用方式

安装后，在对话中明确调用 Skill。例如：

```text
使用 logamee-sdd，先检查当前项目处于哪个 SDD 阶段，再从未完成的阶段继续。
```

```text
使用 logamee-sdd，评估这个测试问题属于实现 Bug、Plan 问题还是规格变更，
并说明判断依据。
```

具体使用边界、工作入口和参考资料见对应 Skill 的
[`README.md`](skills/logamee-sdd/README.md) 和 [`SKILL.md`](skills/logamee-sdd/SKILL.md)。

## 仓库结构

```text
skill-package/
├── README.md
├── CONTRIBUTING.md
├── SECURITY.md
├── CHANGELOG.md
├── THIRD-PARTY-NOTICES.md
└── skills/
    └── logamee-sdd/
        ├── README.md
        ├── SKILL.md
        ├── agents/
        ├── evals/
        └── references/
```

`README.md` 介绍仓库或具体 Skill，`SKILL.md` 是 Agent 实际加载的入口。
`references/` 保存按需读取的详细协议，`agents/` 和 `evals/` 保存可选的
界面元数据与回归评估资料。

## 依赖边界

本仓库提供的是 Agent 指令和参考资料，不捆绑特定模型、运行时、CLI、编辑器
或项目源码。Skill 能否实际修改项目，取决于运行它的 Agent 是否具有对应的
文件访问权限、工具权限和项目上下文。

Skill 不会因为安装到 Agent 目录就自动改变现有项目；具体操作仍由 Agent
根据 Skill 规则、当前项目文件和用户指令执行。

## 贡献

欢迎提交 Issue 和 Pull Request。贡献前请先阅读：

- [贡献指南](CONTRIBUTING.md)
- [安全政策](SECURITY.md)
- 对应 Skill 的 README 和 `SKILL.md`

## 许可状态

Logamee 自有内容的开源许可证尚待维护者确定，因此当前仓库尚未授予通用
再分发或修改许可。第三方材料继续遵循各自附带的许可证，详见
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。
