---
name: to-prd
description: 将当前对话上下文转化为 PRD 并发布到项目问题跟踪器。当用户想要基于当前上下文创建 PRD 时使用。
---

本 skill 获取当前对话上下文和代码库理解，生成一份 PRD。不要采访用户——只需综合你已知的信息。

Issue tracker 和 triage 标签词汇应已提供给你——如果没有，运行 `/setup-matt-pocock-skills`。

## 流程

1. 如果尚未探索仓库，先了解代码库的当前状态。在整个 PRD 中使用项目的领域词汇表词汇，并尊重你触及区域的任何 ADR。

2. 勾画你将测试功能的 seams。现有 seams 应优先于新的。使用尽可能高的 seam。如果需要新的 seams，在尽可能高的位置提出。

与用户确认这些 seams 是否符合他们的期望。

3. 使用下方模板编写 PRD，然后将其发布到项目 issue tracker。应用 `ready-for-agent` triage 标签——无需额外 triage。

<prd-template>

## 问题陈述

用户面临的问题，从用户的角度。

## 解决方案

问题的解决方案，从用户的角度。

## 用户故事

一个很长的编号用户故事列表。每个用户故事应为以下格式：

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

这个用户故事列表应极其详尽，覆盖功能的所有方面。

## 实现决策

已做出的实现决策列表。可以包括：

- 将构建/修改的模块
- 将被修改的模块接口
- 开发者的技术澄清
- 架构决策
- Schema 变更
- API 合约
- 特定交互

不要包含具体的文件路径或代码片段。它们可能很快就会过时。

例外：如果原型产生了一个比散文更精确地编码决策的代码片段（状态机、reducer、schema、类型形状），在相关决策中内联并简要注明它来自原型。裁剪到决策丰富的部分——不是可工作的演示，只是重要的部分。

## 测试决策

已做出的测试决策列表。包括：

- 什么是好测试的描述（只测试外部行为，不测试实现细节）
- 哪些模块将被测试
- 测试的先例（即代码库中类似类型的测试）

## 超出范围

此 PRD 超出范围的事项描述。

## 补充说明

关于该功能的任何补充说明。

</prd-template>
