# Issue tracker：GitHub

此仓库的 issues 和 PRD 作为 GitHub issues 存在。所有操作使用 `gh` CLI。

## 约定

- **创建 issue**：`gh issue create --title "..." --body "..."`。多行 body 使用 heredoc。
- **读取 issue**：`gh issue view <number> --comments`，通过 `jq` 过滤评论并同时获取标签。
- **列出 issues**：`gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'`，配合适当的 `--label` 和 `--state` 过滤器。
- **评论 issue**：`gh issue comment <number> --body "..."`
- **应用 / 移除标签**：`gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **关闭**：`gh issue close <number> --comment "..."`

从 `git remote -v` 推断仓库 —— `gh` 在 clone 内运行时自动执行此操作。

## 当 skill 说"发布到 issue tracker"时

创建 GitHub issue。

## 当 skill 说"获取相关 ticket"时

运行 `gh issue view <number> --comments`。
