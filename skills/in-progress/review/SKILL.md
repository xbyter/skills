---
name: review
description: Review the changes since a fixed point (commit, branch, tag, or merge-base) along two axes — Standards (does the code follow this repo's documented coding standards?) and Spec (does the code match what the originating issue/PRD asked for?). Runs both reviews in parallel sub-agents and reports them side by side. Use when the user wants to review a branch, a PR, work-in-progress changes, or asks to "review since X".
---

# Review

对 `HEAD` 与用户提供的固定点之间的 diff 进行双轴审查：

- **Standards** — 代码是否符合此仓库的文档化编码标准？
- **Spec** — 代码是否忠实实现了源 issue / PRD / spec？

两个轴作为**并行 sub-agents** 运行，这样它们不会互相污染上下文，然后此 skill 汇总它们的发现。

Issue tracker 应已提供给你——如果 `docs/agents/issue-tracker.md` 缺失，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 确定固定点

用户说的就是固定点——commit SHA、分支名、tag、`main`、`HEAD~5` 等。不要挑剔；直接传递。如果他们没指定，问："Review 对比什么——分支、commit 还是 `main`？" 在获得之前不要继续。

一次性捕获 diff 命令：`git diff <fixed-point>...HEAD`（三点，使比较针对 merge-base）。同时通过 `git log <fixed-point>..HEAD --oneline` 记录 commit 列表。

### 2. 识别 spec 来源

按此顺序查找源 spec：

1. Commit 消息中的 issue 引用（`#123`、`Closes #45`、GitLab `!67` 等）——通过 `docs/agents/issue-tracker.md` 中的工作流获取。
2. 用户作为参数传递的路径。
3. `docs/`、`specs/` 或 `.scratch/` 下与分支名或功能匹配的 PRD/spec 文件。
4. 如果什么都没找到，问用户 spec 在哪里。如果他们说没有，**Spec** sub-agent 跳过并报告"no spec available"。

### 3. 识别 standards 来源

仓库中任何记录代码应如何编写的文档。常见位置：

- `CLAUDE.md`、`AGENTS.md`
- `CONTRIBUTING.md`
- `CONTEXT.md`、`CONTEXT-MAP.md`、每个上下文的 `CONTEXT.md` 文件
- `docs/adr/`（架构决策就是标准）
- `.editorconfig`、`eslint.config.*`、`biome.json`、`prettier.config.*`、`tsconfig.json`（机器执行的标准——记录它们但不重新检查工具已检查的内容）
- 仓库根目录或 `docs/` 下的任何 `STYLE.md`、`STANDARDS.md`、`STYLEGUIDE.md` 或类似文件

收集文件列表。**Standards** sub-agent 将阅读它们。

### 4. 并行启动两个 sub-agents

发送单条消息包含两个 `Agent` tool 调用。两个都使用 `general-purpose` subagent。

**Standards sub-agent 提示**——包含：

- 完整的 diff 命令和 commit 列表。
- 你在步骤 3 中找到的 standards 源文件列表。
- 简报："阅读 standards 文档。然后阅读 diff。报告——在相关的地方按文件/hunk——diff 中每个违反文档化标准的地方。引用标准（文件 + 规则）。区分硬违规和判断调用。跳过工具已执行的内容。400 字以内。"

**Spec sub-agent 提示**——包含：

- Diff 命令和 commit 列表。
- Spec 的路径或获取的内容。
- 简报："阅读 spec。然后阅读 diff。报告：(a) spec 要求的缺失或不完整的需求；(b) diff 中未被要求的行为（范围蔓延）；(c) 看起来已实现但实现似乎错误的需求。每个发现引用 spec 行。400 字以内。"

如果 spec 缺失，跳过 Spec sub-agent 并在最终报告中注明。

### 5. 汇总

在 `## Standards` 和 `## Spec` 标题下呈现两份报告，原文或轻度整理。不要合并或重新排序发现——两个轴故意分开，以便用户独立查看。

以一行摘要结束：每个轴的发现总数，以及标记的最严重问题（如有）。

## 为什么是双轴

一个变更可以通过一个轴而失败另一个：

- 遵循所有标准但实现了错误东西的代码 → **Standards 通过，Spec 失败。**
- 完全按照 issue 要求但破坏了项目约定的代码 → **Spec 通过，Standards 失败。**

分开报告可以防止一个轴掩盖另一个。
