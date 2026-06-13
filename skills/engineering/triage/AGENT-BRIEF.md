# 编写 Agent Brief

Agent brief 是当 issue 移至 `ready-for-agent` 时发布在 GitHub issue 上的结构化评论。它是 AFK agent 工作的权威规范。原始 issue 正文和讨论是上下文——agent brief 是合约。

## 原则

### 持久性优于精确性

Issue 可能在 `ready-for-agent` 状态中停留数天或数周。代码库在此期间会发生变化。编写 brief 时要让它在文件被重命名、移动或重构后仍然有用。

- **要**描述接口、类型和行为合约
- **要**命名 agent 应查找或修改的特定类型、函数签名或配置结构
- **不要**引用文件路径——它们会过时
- **不要**引用行号
- **不要**假设当前实现结构会保持不变

### 行为性，而非过程性

描述系统应该**做什么**，而非**怎么做**。Agent 会从头探索代码库并做出自己的实现决策。

- **好：** "`SkillConfig` 类型应接受一个可选的 `schedule` 字段，类型为 `CronExpression`"
- **坏：** "打开 src/types/skill.ts 在第 42 行添加 schedule 字段"
- **好：** "当用户不带参数运行 `/triage` 时，他们应看到需要关注的 issues 摘要"
- **坏：** "在主处理函数中添加 switch 语句"

### 完整的验收标准

Agent 需要知道何时完成。每个 agent brief 必须有具体的、可测试的验收标准。每个标准应可独立验证。

- **好：** "运行 `gh issue list --label needs-triage` 返回已经过初始分类的 issues"
- **坏：** "Triage 应该正常工作"

### 明确的范围边界

声明什么超出范围。这防止 agent 镀金或对相邻功能做出假设。

## 模板

```markdown
## Agent Brief

**Category:** bug / enhancement
**Summary:** 一行描述需要发生什么

**Current behavior:**
描述当前发生什么。对于 bug，这是出错的行为。
对于增强，这是功能构建的基础现状。

**Desired behavior:**
描述 agent 工作完成后应该发生什么。
对边界情况和错误条件要具体。

**Key interfaces:**
- `TypeName` — 需要改什么以及为什么
- `functionName()` 返回类型 — 当前返回什么 vs 应该返回什么
- Config shape — 任何需要的新配置选项

**Acceptance criteria:**
- [ ] 具体的、可测试的标准 1
- [ ] 具体的、可测试的标准 2
- [ ] 具体的、可测试的标准 3

**Out of scope:**
- 不应在此 issue 中更改或处理的事项
- 可能看似相关但实际独立的相邻功能
```

## 示例

### 好的 agent brief（bug）

```markdown
## Agent Brief

**Category:** bug
**Summary:** Skill 描述截断在词中间断开，产生损坏的输出

**Current behavior:**
当 skill 描述超过 1024 字符时，在恰好 1024 字符处截断，
不考虑词边界。这会产生在词中间截断的描述
（例如 "Use when the user wants to confi"）。

**Desired behavior:**
截断应在 1024 字符之前的最后一个词边界处断开
并附加 "..." 以表示截断。

**Key interfaces:**
- `SkillMetadata` 类型的 `description` 字段——无需类型更改，
  但填充它的验证/处理逻辑需要尊重词边界
- 任何读取 SKILL.md frontmatter 并提取描述的函数

**Acceptance criteria:**
- [ ] 1024 字符以下的描述不变
- [ ] 超过 1024 字符的描述在 1024 字符之前的最后一个词边界处截断
- [ ] 截断的描述以 "..." 结尾
- [ ] 包括 "..." 在内的总长度不超过 1024 字符

**Out of scope:**
- 更改 1024 字符限制本身
- 多行描述支持
```

### 好的 agent brief（增强）

```markdown
## Agent Brief

**Category:** enhancement
**Summary:** 添加 `.out-of-scope/` 目录支持以跟踪被拒绝的功能请求

**Current behavior:**
当功能请求被拒绝时，issue 以 `wontfix` 标签和评论关闭。
没有决策或推理的持久记录。
未来类似的请求需要维护者回忆或搜索之前的讨论。

**Desired behavior:**
被拒绝的功能请求应记录在 `.out-of-scope/<concept>.md` 文件中，
捕获决策、推理和所有请求该功能的 issues 链接。
在 triage 新 issues 时，应检查这些文件以进行匹配。

**Key interfaces:**
- `.out-of-scope/` 中的 Markdown 文件格式——每个文件应有
  `# Concept Name` 标题、`**Decision:**` 行、`**Reason:**` 行、
  和带 issue 链接的 `**Prior requests:**` 列表
- Triage 工作流应尽早读取所有 `.out-of-scope/*.md` 文件
  并按概念相似性将传入 issues 与之匹配

**Acceptance criteria:**
- [ ] 以 wontfix 关闭功能时创建/更新 `.out-of-scope/` 中的文件
- [ ] 文件包含决策、推理和已关闭 issue 的链接
- [ ] 如果匹配的 `.out-of-scope/` 文件已存在，新 issue 被
      追加到其 "Prior requests" 列表而非创建副本
- [ ] 在 triage 期间，当新 issue 匹配之前的拒绝时，
      检查并呈现现有的 `.out-of-scope/` 文件

**Out of scope:**
- 自动匹配（人工确认匹配）
- 重新打开之前被拒绝的功能
- Bug 报告（只有增强拒绝进入 `.out-of-scope/`）
```

### 坏的 agent brief

```markdown
## Agent Brief

**Summary:** 修复 triage bug

**What to do:**
Triage 的东西坏了。看看主文件修复它。
大约第 150 行的函数有问题。

**Files to change:**
- src/triage/handler.ts (line 150)
- src/types.ts (line 42)
```

这很坏因为：
- 没有 category
- 模糊描述（"triage 的东西坏了"）
- 引用会过时的文件路径和行号
- 没有验收标准
- 没有范围边界
- 没有当前 vs 期望行为的描述
