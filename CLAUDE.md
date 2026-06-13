Skills 按类别组织在 `skills/` 下的分桶文件夹中：

- `engineering/` — 日常代码工作
- `productivity/` — 日常非代码工作流工具
- `misc/` — 保留但很少使用
- `personal/` — 与个人环境绑定，不推广
- `in-progress/` — 尚未准备好发布的草稿
- `deprecated/` — 不再使用

`engineering/`、`productivity/` 或 `misc/` 中的每个 skill 都必须在顶层 `README.md` 中有引用，并在 `.claude-plugin/plugin.json` 中有对应条目。`personal/`、`in-progress/` 和 `deprecated/` 中的 skill 不得出现在这两者中。

顶层 `README.md` 中的每个 skill 条目必须将 skill 名称链接到其 `SKILL.md`。

每个分桶文件夹都有一个 `README.md`，列出该分桶中的每个 skill 及一行描述，skill 名称链接到其 `SKILL.md`。
