# 仅对硬依赖显式提供 `/setup-matt-pocock-skills` 指引

Engineering skills 依赖于由 `/setup-matt-pocock-skills` 生成的按仓库配置（issue tracker、triage 标签词汇、领域文档布局）。有些 skills 没有该配置就无法正常运行 —— 它们必须发布到特定的 issue tracker 或应用特定的标签字符串。其他 skills 只是用它来优化输出（词汇、ADR 感知），没有它也能优雅降级。

我们将这些分为 **硬依赖** 和 **软依赖** skills：

- **硬依赖**（`to-issues`、`to-prd`、`triage`）—— 包含显式的一句话提示：_"……应该已经提供给你了 —— 如果没有，请运行 `/setup-matt-pocock-skills`。"_ 没有映射，输出就是错的，而不仅仅是模糊。
- **软依赖**（`diagnose`、`tdd`、`improve-codebase-architecture`、`zoom-out`）—— 仅在模糊的文本中引用"项目的领域术语表"和"你正在接触的区域的 ADR"。如果文档不存在，skill 仍然有效；只是输出不够精准。

这种划分使软依赖 skills 保持轻量 token，并避免将 setup 指引复制到它不起关键作用的地方。
