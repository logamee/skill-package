# Logamee Skill Package

Logamee 维护的 Agent Skills 合集。每个 Skill 都是独立目录，可按需安装；当前收录：

| Skill | 用途 |
| --- | --- |
| [`logamee-sdd`](skills/logamee-sdd/README.md) | 推进软件项目从需求澄清、规格与计划，到实现、测试、验收和后续变更的 SDD 流程。 |

## 安装

只安装需要的 Skill，不必把整个合集复制到 Agent 的 Skill 目录。以 `logamee-sdd` 为例，将 [`skills/logamee-sdd/`](skills/logamee-sdd/) 复制或链接到 Agent 所使用的 Skill 目录。目录内必须保留 `SKILL.md`、`references/`、`agents/` 和 `evals/`。

```bash
git clone https://github.com/logamee/skill-package.git
```

各 Agent 的发现目录和安装方式可能不同，请按对应 Agent 的配置选择目标目录。Skill 采用 `SKILL.md` 加可选参考资料的目录形式；`agents/openai.yaml` 是附加的 OpenAI/Codex 界面元数据，不是运行依赖。

Codex 示例（在仓库根目录执行）：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/logamee-sdd "${CODEX_HOME:-$HOME/.codex}/skills/"
```

## 目录结构

```text
skill-package/
├── README.md
└── skills/
    └── logamee-sdd/
        ├── SKILL.md
        ├── README.md
        ├── agents/
        ├── evals/
        └── references/
```

## 许可状态

Logamee 自有内容的开源许可证尚待维护者确定，因此当前仓库尚未授予通用再分发或修改许可。第三方材料继续遵循各自附带的许可证，详见 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。

## 贡献

贡献流程见 [CONTRIBUTING.md](CONTRIBUTING.md)。提交问题前，请先阅读对应 Skill 的说明和适用范围。
