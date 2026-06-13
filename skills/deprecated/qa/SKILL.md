---
name: qa
description: Interactive QA session where user reports bugs or issues conversationally, and the agent files GitHub issues. Explores the codebase in the background for context and domain language. Use when user wants to report bugs, do QA, file issues conversationally, or mentions "QA session".
---

# QA 会话

运行交互式 QA 会话。用户描述他们遇到的问题。你进行澄清，在后台探索代码库获取上下文，然后提交持久、面向用户且使用项目领域语言的 GitHub issues。

## 对于用户提出的每个 issue

### 1. 倾听并轻度澄清

让用户用自己的话描述问题。问**最多 2-3 个简短的澄清问题**，聚焦于：

- 他们的期望 vs 实际发生了什么
- 复现步骤（如果不明显的话）
- 是持续出现还是间歇性的

不要过度采访。如果描述足够清晰可以提交，继续前进。

### 2. 在后台探索代码库

在与用户交谈的同时，在后台启动一个 Agent（subagent_type=Explore）以了解相关区域。目标不是找到修复方案——而是：

- 学习该区域使用的领域语言（检查 UBIQUITOUS_LANGUAGE.md）
- 理解功能应该做什么
- 识别面向用户的行为边界

这些上下文帮助你写出更好的 issue——但 issue 本身不应引用特定文件、行号或内部实现细节。

### 3. 评估范围：单个 issue 还是拆分？

提交前，决定这是**单个 issue**还是需要**拆分为多个 issues。

拆分条件：

- 修复跨越多个独立区域（例如"表单验证有误 AND 成功消息缺失 AND 重定向坏了"）
- 有明确可分离的关注点，不同的人可以并行工作
- 用户描述的内容有多个不同的失败模式或症状

保持为单个 issue 的条件：

- 一个行为在一个地方出了问题
- 所有症状都由同一个根本行为引起

### 4. 提交 GitHub issue(s)

使用 `gh issue create` 创建 issues。不要先让用户审查——直接提交并分享 URL。

Issues 必须**持久**——它们在大规模重构后仍应有意义。从用户的角度编写。

#### 单个 issue

使用此模板：

```
## What happened

[用简单语言描述用户经历的实际行为]

## What I expected

[描述期望的行为]

## Steps to reproduce

1. [开发者可以跟随的具体编号步骤]
2. [使用代码库中的领域术语，而非内部模块名]
3. [包含相关输入、标志或配置]

## Additional context

[来自用户或代码库探索的任何额外观察，帮助框定问题——例如"这只在使用 Docker 层时发生，而非文件系统层"——使用领域语言但不引用文件]
```

#### 拆分（多个 issues）

按依赖顺序创建 issues（阻塞者在前），这样你可以引用真实的 issue 编号。

每个子 issue 使用此模板：

```
## Parent issue

#<parent-issue-number>（如果创建了跟踪 issue）或 "Reported during QA session"

## What's wrong

[描述这个特定的行为问题——只是这个切片，不是整个报告]

## What I expected

[这个特定切片的期望行为]

## Steps to reproduce

1. [特定于此 issue 的步骤]

## Blocked by

- #<issue-number>（如果此 issue 在另一个修复前无法被修复）

或 "None — can start immediately" 如果没有阻塞者。

## Additional context

[与此切片相关的任何额外观察]
```

创建拆分时：

- **优先多个薄 issues 而非少量厚 ones**——每个应可独立修复和验证
- **诚实地标记阻塞关系**——如果 issue B 确实无法在 issue A 修复前测试，说明。如果它们独立，都标记为"None — can start immediately"
- **按依赖顺序创建 issues** 以便在"Blocked by"中引用真实 issue 编号
- **最大化并行性**——目标是多个人（或 agents）可以同时接手不同的 issues

#### 所有 issue 正文的规则

- **不要文件路径或行号**——它们会过时
- **使用项目的领域语言**（如存在则检查 UBIQUITOUS_LANGUAGE.md）
- **描述行为，而非代码**——"同步服务未能应用补丁"而非"applyPatch() 在第 42 行抛出"
- **复现步骤是必须的**——如果无法确定，问用户
- **保持简洁**——开发者应能在 30 秒内阅读完 issue

提交后，打印所有 issue URL（附阻塞关系摘要）并问："下一个问题，还是完成了？"

### 5. 继续会话

继续直到用户说完成了。每个 issue 独立——不要批量处理。
