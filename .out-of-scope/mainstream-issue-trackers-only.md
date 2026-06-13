# Issue tracker 集成仅限于主流工具

`setup-matt-pocock-skills` 仅为**主流** issue trackers 提供一等支持。添加对小众、新兴或单一供应商实验性 trackers 支持的请求超出范围。

## 为什么这超出范围

每个 issue-tracker 后端都在 skills 中硬编码了 CLI 结构（命令、标志、输出解析）。每个新后端都是永久的维护面——它必须在工具的 CLI 演变时保持工作，并且必须持续针对 `/to-prd`、`/to-issues`、`/triage` 等进行测试。这个成本只有在对相当比例用户实际使用的 trackers 上才值得投入。

"主流"是一个判断性调用，而非数字门槛：

- GitHub、GitLab 和 Backlog.md 是我们认为主流的工具——广为人知、广泛使用、早已过了实验阶段。
- 一个全新的面向 agent 的工具，即使设计多么有趣，只有几百个 GitHub stars，也不算。

Stars、年龄和下载量在做判断时是有用的信号，但它们都不是规则。规则是：一个普通工程师能否认出这个工具并合理地为其团队选择它？

非主流 trackers 的逃生通道已经存在：

- `local markdown` 用于轻量级仓库内跟踪。
- `other/custom` 供想自己接入的用户使用。

两者都不需要核心 skills 了解特定工具。

## 之前的请求

- #99 — "Add dex as an issue tracker backend"（dex 在请求时约 3 个月大，约 300 stars）
