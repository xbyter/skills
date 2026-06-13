---
name: to-issues
description: Break a plan, spec, or PRD into independently-grabbable issues on the project issue tracker using tracer-bullet vertical slices. Use when user wants to convert a plan into issues, create implementation tickets, or break down work into issues.
---

# 转为 Issues

使用垂直切片（tracer bullets）将计划拆分为可独立获取的 issues。

Issue tracker 和 triage 标签词汇应已提供给你——如果没有，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 收集上下文

基于对话上下文中已有的内容工作。如果用户传递了 issue 引用（issue 编号、URL 或路径）作为参数，从 issue tracker 获取并阅读其完整内容和评论。

### 2. 探索代码库（可选）

如果你尚未探索代码库，请了解代码的当前状态。Issue 标题和描述应使用项目的领域词汇表词汇，并尊重你触及区域的 ADR。

### 3. 起草垂直切片

将计划拆分为 **tracer bullet** issues。每个 issue 是一个薄的垂直切片，端到端贯穿所有集成层，而不是某一层的水平切片。

切片可以是 'HITL' 或 'AFK'。HITL 切片需要人工交互，如架构决策或设计评审。AFK 切片可以在无人工交互的情况下实现和合并。尽可能优先选择 AFK 而非 HITL。

<vertical-slice-rules>
- 每个切片交付一条窄但完整的路径，贯穿每一层（schema、API、UI、测试）
- 完成的切片可独立演示或验证
- 优先选择多个薄切片而非少量厚切片
</vertical-slice-rules>

### 4. 询问用户

将提议的拆分以编号列表呈现。对于每个切片，展示：

- **标题**：简短描述性名称
- **类型**：HITL / AFK
- **被阻塞**：哪些其他切片（如有）必须先完成
- **覆盖的用户故事**：这解决了哪些用户故事（如果源材料有的话）

问用户：

- 粒度感觉对吗？（太粗/太细）
- 依赖关系正确吗？
- 是否有切片需要进一步合并或拆分？
- HITL 和 AFK 标记正确吗？

迭代直到用户批准拆分方案。

### 5. 将 issues 发布到 issue tracker

对于每个批准的切片，向 issue tracker 发布一个新 issue。使用下方的 issue 正文模板。这些 issues 被视为可供 AFK agents 使用，因此除非另有指示，使用正确的 triage 标签发布。

按依赖顺序发布 issues（阻塞者在前），这样你可以在"Blocked by"字段中引用真实的 issue 标识符。

<issue-template>
## Parent

对 issue tracker 上父 issue 的引用（如果来源是现有 issue，否则省略此部分）。

## 要构建什么

这个垂直切片的简洁描述。描述端到端行为，而非逐层实现。

避免具体的文件路径或代码片段——它们很快就会过时。例外：如果原型产生了一个比散文更精确地编码决策的代码片段（状态机、reducer、schema、类型形状），在此内联并简要注明它来自原型。裁剪到决策丰富的部分——不是可工作的演示，只是重要的部分。

## 验收标准

- [ ] 标准 1
- [ ] 标准 2
- [ ] 标准 3

## Blocked by

- 阻塞 ticket 的引用（如有）

或"无 - 可以立即开始"如果没有阻塞者。

</issue-template>

不要关闭或修改任何父 issue。
