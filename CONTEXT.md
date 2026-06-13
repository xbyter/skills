# Matt Pocock Skills

一组由 Claude Code 加载的 agent skills（斜杠命令和行为）。Skills 按分桶组织，由 `/setup-matt-pocock-skills` 生成的按仓库配置来消费。

## 术语

**Issue tracker（问题追踪器）**：
托管仓库 issues 的工具 — GitHub Issues、Linear、本地 `.scratch/` markdown 约定或类似工具。`to-issues`、`to-prd`、`triage` 和 `qa` 等 skills 会从中读取和写入。
_避免使用_：backlog manager、backlog backend、issue host

**Issue（问题）**：
**Issue tracker** 中的单个跟踪工作单元 — 一个 bug、任务、PRD 或由 `to-issues` 产出的切片。
_避免使用_：ticket（仅在引用将其称为 tickets 的外部系统时使用）

**Triage role（分诊角色）**：
在 triage 过程中应用于 **Issue** 的规范状态机标签（例如 `needs-triage`、`ready-for-afk`）。每个角色通过 `docs/agents/triage-labels.md` 映射到 **Issue tracker** 中的实际标签字符串。

## 关系

- 一个 **Issue tracker** 包含多个 **Issues**
- 一个 **Issue** 同一时间承载一个 **Triage role**

## 已标记的歧义

- "backlog" 之前被用来指代托管 issues 的*工具*和其中的*工作总量* — 已解决：该工具称为 **Issue tracker**；"backlog" 不再作为领域术语使用。
- "backlog backend" / "backlog manager" — 已解决：统一为 **Issue tracker**。
