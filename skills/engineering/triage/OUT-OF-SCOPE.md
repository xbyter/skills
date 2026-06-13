# 超出范围知识库

仓库中的 `.out-of-scope/` 目录存储被拒绝功能请求的持久记录。它有两个目的：

1. **机构记忆** — 为什么某个功能被拒绝，这样当 issue 关闭后推理不会丢失
2. **去重** — 当新 issue 匹配之前的拒绝时，skill 可以呈现之前的决策而非重新讨论

## 目录结构

```
.out-of-scope/
├── dark-mode.md
├── plugin-system.md
└── graphql-api.md
```

每个**概念**一个文件，而非每个 issue。请求同一事物的多个 issue 归入一个文件。

## 文件格式

文件应以轻松、可读的风格编写——更像简短的设计文档而非数据库条目。使用段落、代码示例和示例使推理对首次接触它的人清晰有用。

```markdown
# Dark Mode

本项目不支持 dark mode 或面向用户的主题。

## 为什么这超出范围

渲染管道假设在 `ThemeConfig` 中定义单一调色板。
支持多主题需要：

- 包裹整个组件树的 theme context provider
- 每个组件的主题感知样式解析
- 用户主题偏好的持久化层

这是一个重大的架构变更，与项目关注内容创作的方向不一致。
主题是下游消费者的关注点，他们嵌入或重新分发输出。

```ts
// 当前 ThemeConfig 接口不是为运行时切换设计的：
interface ThemeConfig {
  colors: ColorPalette; // 单一调色板，在构建时解析
  fonts: FontStack;
}
```

## 之前的请求

- #42 — "Add dark mode support"
- #87 — "Night theme for accessibility"
- #134 — "Dark theme option"
```

### 命名文件

为概念使用简短、描述性的 kebab-case 名称：`dark-mode.md`、`plugin-system.md`、`graphql-api.md`。名称应足够容易辨认，让浏览目录的人无需打开文件就能理解什么被拒绝了。

### 编写原因

原因应是实质性的——不是"我们不想要这个"而是为什么。好的原因引用：

- 项目范围或理念（"本项目专注于 X；主题是下游关注点"）
- 技术约束（"支持这需要 Y，这与我们的 Z 架构冲突"）
- 战略决策（"我们选择使用 A 而非 B 因为..."）

原因应是持久的。避免引用临时情况（"我们现在太忙了"）——那些不是真正的拒绝，是推迟。

## 何时检查 `.out-of-scope/`

在 triage 期间（步骤 1：收集上下文），读取 `.out-of-scope/` 中的所有文件。评估新 issue 时：

- 检查请求是否匹配已有的超出范围概念
- 匹配基于概念相似性，而非关键词——"night theme"匹配 `dark-mode.md`
- 如有匹配，向维护者呈现："这类似于 `.out-of-scope/dark-mode.md`——我们之前因为这个原因拒绝了。你仍然这样认为吗？"

维护者可以：

- **确认** — 新 issue 被添加到现有文件的"Prior requests"列表，然后关闭
- **重新考虑** — 超出范围文件被删除或更新，issue 进入正常 triage 流程
- **不同意** — issues 相关但不同，进入正常 triage

## 何时写入 `.out-of-scope/`

仅当**增强**（非 bug）被以 `wontfix` 拒绝时。流程：

1. 维护者决定功能请求超出范围
2. 检查是否已有匹配的 `.out-of-scope/` 文件
3. 如果有：将新 issue 追加到"Prior requests"列表
4. 如果没有：创建新文件，包含概念名称、决策、原因和第一个 prior request
5. 在 issue 上发评论解释决策并提及 `.out-of-scope/` 文件
6. 以 `wontfix` 标签关闭 issue

## 更新或删除超出范围文件

如果维护者改变了对之前拒绝概念的想法：

- 删除 `.out-of-scope/` 文件
- Skill 不需要重新打开旧 issues——它们是历史记录
- 触发重新考虑的新 issue 进入正常 triage 流程
