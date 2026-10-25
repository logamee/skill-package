# Logamee SDD

一个面向软件项目的 Agent Skill，统一推进需求讨论、首次开发、Bootstrap 和稳定版本变更。它会结合当前仓库资料选择后续路径，并区分实现 Bug、Plan 调整、已有规格变更、新规格及混合变更。

## 使用

把本目录安装到 Agent 的 Skill 目录，并在对话中提出开发任务，例如：

Codex 示例（在合集仓库根目录执行）：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/logamee-sdd "${CODEX_HOME:-$HOME/.codex}/skills/"
```

```text
使用 logamee-sdd，先检查当前项目处于哪个 SDD 阶段，再从未完成的阶段继续。
```

```text
使用 logamee-sdd，评估这个测试问题属于实现 Bug、Plan 问题还是规格变更，并说明判断依据。
```

Skill 会先检查当前项目文件和实现状态；不会仅凭历史会话假定项目阶段。具体分流规则、阶段约束和参考资料入口见 [`SKILL.md`](SKILL.md)。

## 目录内容

- `SKILL.md`：入口、模式分流和共同规则。
- `references/`：按需读取的主流程、变更、Bootstrap、访谈和项目目录约定。
- `agents/openai.yaml`：OpenAI/Codex 界面元数据。
- `evals/evals.json`：分流行为的回归提示词。

本 Skill 不附带可执行程序，也不要求安装特定 CLI。实施阶段能否操作项目，取决于运行它的 Agent 是否具有相应的文件和工具权限。

## 许可状态

Logamee 自有内容的开源许可证尚待维护者确定。第三方快照的来源和许可见仓库根目录的 [THIRD-PARTY-NOTICES.md](../../THIRD-PARTY-NOTICES.md)。
