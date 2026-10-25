# Source Map

本文件记录方法来源和参考快照，供维护者追溯。实际执行入口是 `../SKILL.md` 及其链接的本地协议。

| 内容 | 来源 | 固定版本 / 说明 |
| --- | --- | --- |
| Grill Me / Grilling 访谈参考 | [`mattpocock/skills`](https://github.com/mattpocock/skills) | `c55ee46073ed923f86ce59a5eb3b6d895095d1b7`；快照位于 `upstream/grill-me/`、`upstream/grilling/`。 |
| Specify、Plan、Tasks、Implement 命令参考 | [`github/spec-kit`](https://github.com/github/spec-kit) | `adbd62af15f363cbaf1e69e117eb8444d525a0a0`；快照位于 `upstream/spec-kit/commands/`。 |
| BugFix 与 Change Flow 历史参考 | Logamee SDD 维护材料 | 快照位于 `upstream/speckit-bugfix/`、`upstream/speckit-change-flow/`；当前执行规则以本 Skill 的整合协议为准。 |
| SDD 分流、Bootstrap、验收和项目目录约定 | Logamee SDD 方法整理 | 根据软件项目实践整理，不要求其他项目采用相同的编号、版本或目录命名。 |

快照用于追溯和比对，不是并列执行入口。上游模板可能包含其他 Agent 平台的命令、钩子或脚本约定；只有本地协议明确要求且当前环境具备相应工具时才采用。不得让快照覆盖本 Skill 的用户确认、项目文档和变更流程规则。
