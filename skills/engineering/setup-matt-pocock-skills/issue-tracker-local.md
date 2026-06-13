# Issue tracker: 本地 Markdown

本仓库的 issues 和 PRD 以 markdown 文件形式存放在 `.scratch/` 目录中。

## 约定

- 每个功能一个目录：`.scratch/<feature-slug>/`
- PRD 文件为 `.scratch/<feature-slug>/PRD.md`
- 实现 issues 为 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始编号
- Triage 状态记录在每个 issue 文件顶部的 `Status:` 行中（角色字符串参见 `triage-labels.md`）
- 评论和对话历史追加到文件底部的 `## Comments` 标题下

## 当 skill 说"发布到 issue tracker"时

在 `.scratch/<feature-slug>/` 下创建新文件（如需要则创建目录）。

## 当 skill 说"获取相关 ticket"时

读取引用路径的文件。用户通常会直接传递路径或 issue 编号。
