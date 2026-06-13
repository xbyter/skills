---
name: setup-matt-pocock-skills
description: Sets up an `## Agent skills` block in AGENTS.md/CLAUDE.md and `docs/agents/` so the engineering skills know this repo's issue tracker (GitHub or local markdown), triage label vocabulary, and domain doc layout. Run before first use of `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out` — or if those skills appear to be missing context about the issue tracker, triage labels, or domain docs.
disable-model-invocation: true
---

# 设置 Matt Pocock 的 Skills

搭建 engineering skills 假定的按仓库配置：

- **Issue tracker** —— issues 的位置（默认 GitHub；也开箱支持本地 markdown）
- **Triage 标签** —— 五个规范 triage role 使用的字符串
- **领域文档** —— `CONTEXT.md` 和 ADR 的位置，以及消费它们的规则

这是一个提示驱动的 skill，而非确定性脚本。探索、展示你找到的、与用户确认、然后写入。

## 流程

### 1. 探索

查看当前仓库以了解其起始状态。读取任何存在的内容；不要假设：

- `git remote -v` 和 `.git/config` —— 这是 GitHub 仓库吗？哪个？
- 仓库根的 `AGENTS.md` 和 `CLAUDE.md` —— 任何一个存在吗？其中是否已有 `## Agent skills` 部分？
- 仓库根的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录
- `docs/agents/` —— 此 skill 之前的输出是否已存在？
- `.scratch/` —— 表明本地 markdown issue tracker 约定已在使用中

### 2. 展示发现并询问

总结什么存在什么缺失。然后引导用户**逐一**完成三个决策 —— 展示一个部分，获得用户的答案，然后进入下一个。不要一次倾倒所有三个。

假设用户不知道这些术语是什么意思。每个部分以简短的解释开始（它是什么、为什么这些 skills 需要它、如果选择不同的会怎样）。然后展示选项和默认值。

**部分 A —— Issue tracker。**

> 解释："issue tracker" 是此仓库的 issues 所在位置。`to-issues`、`triage`、`to-prd` 和 `qa` 等 skills 从中读取和写入 —— 它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写 markdown 文件，还是遵循你描述的其他工作流。选择你实际跟踪此仓库工作的地方。

默认姿态：这些 skills 是为 GitHub 设计的。如果 `git remote` 指向 GitHub，建议该选项。如果 `git remote` 指向 GitLab（`gitlab.com` 或自托管主机），建议 GitLab。否则（或如果用户偏好），提供：

- **GitHub** —— issues 在仓库的 GitHub Issues 中（使用 `gh` CLI）
- **GitLab** —— issues 在仓库的 GitLab Issues 中（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown** —— issues 作为 `.scratch/<feature>/` 下的文件（适合独立项目或无远程的仓库）
- **其他**（Jira、Linear 等）—— 请用户用一段描述工作流；skill 将其记录为自由格式文本

**部分 B —— Triage 标签词汇。**

> 解释：当 `triage` skill 处理一个传入的 issue 时，它通过状态机移动它 —— 需要评估、等待报告者、准备好让 AFK agent 领取、准备好让人类处理、或不修复。为此，它需要应用与你*实际配置的*字符串匹配的标签。如果你的仓库已使用不同的标签名（例如 `bug:triage` 而非 `needs-triage`），在此映射以便 skill 应用正确的标签而非创建重复的。

五个规范角色：

- `needs-triage` —— 维护者需要评估
- `needs-info` —— 等待报告者
- `ready-for-agent` —— 完全指定，AFK 就绪（agent 可以在没有人类上下文的情况下领取）
- `ready-for-human` —— 需要人类实现
- `wontfix` —— 不会被处理

默认：每个角色的字符串等于其名称。询问用户是否想覆盖任何。如果他们的 issue tracker 没有现有标签，默认就行。

**部分 C —— 领域文档。**

> 解释：一些 skills（`improve-codebase-architecture`、`diagnose`、`tdd`）读取 `CONTEXT.md` 文件来学习项目的领域语言，以及 `docs/adr/` 来了解过去的架构决策。它们需要知道仓库有一个全局上下文还是多个（例如有独立前端/后端上下文的 monorepo），以便在正确的位置查找。

确认布局：

- **单上下文** —— 仓库根一个 `CONTEXT.md` + `docs/adr/`。大多数仓库是这样。
- **多上下文** —— 根目录的 `CONTEXT-MAP.md` 指向每个上下文的 `CONTEXT.md` 文件（通常是 monorepo）。

### 3. 确认并编辑

向用户展示草稿：

- 要添加到 `CLAUDE.md` / `AGENTS.md` 中正在编辑的那个的 `## Agent skills` 块（选择规则见步骤 4）
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容

让他们在写入前编辑。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果都不存在，问用户创建哪个 —— 不要替他们选择。

永远不要在 `CLAUDE.md` 已存在时创建 `AGENTS.md`（反之亦然）—— 始终编辑已存在的那个。

如果所选文件中已存在 `## Agent skills` 块，就地更新其内容而非追加重复项。不要覆盖用户对周围部分的编辑。

该块：

```markdown
## Agent skills

### Issue tracker

[一行总结 issues 在哪里跟踪]。参见 `docs/agents/issue-tracker.md`。

### Triage labels

[一行总结标签词汇]。参见 `docs/agents/triage-labels.md`。

### Domain docs

[一行总结布局 —— "单上下文" 或 "多上下文"]。参见 `docs/agents/domain.md`。
```

然后使用此 skill 文件夹中的种子模板作为起点写入三个 docs 文件：

- [issue-tracker-github.md](./issue-tracker-github.md) —— GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) —— GitLab issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md) —— 本地 markdown issue tracker
- [triage-labels.md](./triage-labels.md) —— 标签映射
- [domain.md](./domain.md) —— 领域文档消费规则 + 布局

对于"其他" issue tracker，使用用户的描述从头编写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户设置完成以及哪些 engineering skills 现在会从这些文件中读取。提到他们以后可以直接编辑 `docs/agents/*.md` —— 只有在想切换 issue tracker 或从头开始时才需要重新运行此 skill。
