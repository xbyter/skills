# `setup-matt-pocock-skills` 的验证/检查模式

本项目不会为 `setup-matt-pocock-skills` 添加专用的验证/检查模式（或单独的验证 skill）。

## 为什么这超出范围

第二个 skill——或 `--verify` 标志——用于检查 `docs/agents/*.md` 工件是否仍匹配种子模板 schema，这会重复现有 setup skill 已在对话中处理的工作。

预期的工作流是：**运行 `/setup-matt-pocock-skills` 并告诉它验证你的当前设置。** 该 skill 是提示驱动的，因此维护者可以将其范围限定为验证通过（"不要重写任何东西，只需根据当前种子模板检查我的现有文件并报告偏差"），而无需单独的代码路径。添加标志或兄弟 skill 会分裂一个已经可以通过自然语言入口点表达的功能的表面积。

将配置管理保持在单一 skill 中也避免了两个 skills 在种子模板演变时互相偏离的维护成本。

## 之前的请求

- #106 — Feature request: verify/check mode for setup-matt-pocock-skills
