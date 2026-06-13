---
name: Obsidian知识库
description: 在 Obsidian 知识库中使用 wiki 链接和索引笔记进行搜索、创建和管理笔记。当用户想要在 Obsidian 中查找、创建或组织笔记时使用。
---

# Obsidian Vault

## Vault 位置

`/mnt/d/Obsidian Vault/AI Research/`

根级别大部分是扁平的。

## 命名约定

- **Index notes**: 聚合相关主题（例如 `Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
- 所有笔记名称使用**Title Case**
- 不使用文件夹组织——用链接和 index notes 代替

## 链接

- 使用 Obsidian `[[wikilinks]]` 语法：`[[Note Title]]`
- 笔记在底部链接到依赖/相关笔记
- Index notes 只是 `[[wikilinks]]` 列表

## 工作流

### 搜索笔记

```bash
# 按文件名搜索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 按内容搜索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或直接在 vault 路径上使用 Grep/Glob 工具。

### 创建新笔记

1. 文件名使用 **Title Case**
2. 将内容写为一个学习单元（按 vault 规则）
3. 在底部添加 `[[wikilinks]]` 到相关笔记
4. 如果是编号序列的一部分，使用层级编号方案

### 查找相关笔记

在 vault 中搜索 `[[Note Title]]` 以查找反向链接：

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找 index notes

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
