---
name: write-a-skill
description: 创建具有正确结构、渐进式披露和捆绑资源的新代理技能。当用户想要创建、编写或构建新技能时使用。
---

# 编写 Skills

## 流程

1. **收集需求** - 问用户：
   - Skill 覆盖什么任务/领域？
   - 应处理什么具体用例？
   - 需要可执行脚本还是仅指令？
   - 有要包含的参考资料吗？

2. **起草 skill** - 创建：
   - 带简洁指令的 SKILL.md
   - 如内容超过 500 行则添加额外参考文件
   - 如需要确定性操作则添加工具脚本

3. **与用户审查** - 呈现草稿并问：
   - 这覆盖你的用例吗？
   - 有缺失或不清楚的吗？
   - 有任何部分需要更多/更少细节吗？

## Skill 结构

```
skill-name/
├── SKILL.md           # 主要指令（必需）
├── REFERENCE.md       # 详细文档（如需要）
├── EXAMPLES.md        # 使用示例（如需要）
└── scripts/           # 工具脚本（如需要）
    └── helper.js
```

## SKILL.md 模板

```md
---
name: skill-name
description: 能力简述。当 [特定触发条件] 时使用。
---

# Skill Name

## Quick start

[最小可工作示例]

## Workflows

[带复杂任务检查清单的逐步流程]

## Advanced features

[链接到单独文件：参见 [REFERENCE.md](REFERENCE.md)]
```

## 描述要求

描述是**你的 agent 在决定加载哪个 skill 时看到的唯一内容**。它与所有其他已安装 skills 一起在系统提示中呈现。你的 agent 阅读这些描述并根据用户的请求选择相关 skill。

**目标**：给你的 agent 刚好足够的信息以了解：

1. 此 skill 提供什么能力
2. 何时/为什么触发它（特定关键词、上下文、文件类型）

**格式**：

- 最多 1024 字符
- 第三人称编写
- 第一句：它做什么
- 第二句："当 [特定触发条件] 时使用"

**好示例**：

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.
```

**坏示例**：

```
Helps with documents.
```

坏示例让你的 agent 无法将此与其他文档 skills 区分。

## 何时添加脚本

在以下情况添加工具脚本：

- 操作是确定性的（验证、格式化）
- 相同代码会被重复生成
- 错误需要显式处理

脚本比生成的代码节省 tokens 并提高可靠性。

## 何时拆分文件

在以下情况拆分为单独文件：

- SKILL.md 超过 100 行
- 内容有不同的领域（财务 vs 销售 schemas）
- 高级功能很少需要

## 审查检查清单

起草后验证：

- [ ] 描述包含触发条件（"Use when..."）
- [ ] SKILL.md 在 100 行以内
- [ ] 无时间敏感信息
- [ ] 术语一致
- [ ] 包含具体示例
- [ ] 引用一层深
