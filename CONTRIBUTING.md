# Contributing

感谢参与维护 Logamee Skill Package。

## 提交变更

1. 一个 Skill 放在 `skills/<skill-name>/`，目录名使用小写 kebab-case。
2. 每个 Skill 必须包含名称与用途匹配的 `SKILL.md`，并将详细流程放在确有需要的 `references/` 中。
3. 修改分流或关键行为时，在 `evals/evals.json` 增加或调整相应回归案例；同步更新该 Skill 的 README。
4. 新增或改写第三方材料时，记录来源、固定版本和许可，并保留原始许可文件。
5. PR 说明变更目的、影响的 Skill、验证方式和未验证事项。

## 本地检查

- 确认 `SKILL.md` frontmatter 中的 `name` 与目录名一致。
- 确认所有相对引用都指向仓库中存在的文件。
- 对照 `evals/evals.json` 的案例检查相关路由行为。

仅改文档时不要求增加不相关的自动化基础设施。
