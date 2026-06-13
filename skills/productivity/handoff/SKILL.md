---
name: handoff
description: 将当前对话压缩为一份交接文档，供另一个代理接手。
argument-hint: "What will the next session be used for?"
---

编写交接文档总结当前对话，以便新的 agent 可以继续工作。保存到用户操作系统的临时目录——不是当前工作区。

在文档中包含"suggested skills"部分，推荐 agent 应调用的 skills。

不要重复已在其他工件中捕获的内容（PRDs、计划、ADRs、issues、commits、diffs）。通过路径或 URL 引用它们。

编辑任何敏感信息，如 API 密钥、密码或个人身份信息。

如果用户传递了参数，将其视为对下一个会话关注点的描述，并相应定制文档。
