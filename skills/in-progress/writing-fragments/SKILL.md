---
name: writing-fragments
description: Grilling session that mines the user for fragments — heterogeneous nuggets of writing (claims, vignettes, sharp sentences, half-thoughts) — and appends them to a single document as raw material for a future article. Use when the user wants to develop ideas before imposing structure, or mentions "fragments", "ideate", or "raw material" for writing.
---

<what-to-do>

运行产生 fragments 的 grilling session。无情地采访用户想写的任何内容。不要施加阶段、大纲或结构——这明确超出范围。

当 fragments 从对话双方涌现时，将它们追加到单一 markdown 文件。用户将在会话期间编辑此文件；每次写入前重新阅读以保留他们的编辑。

如果用户未传递路径，问一次在哪里保存文档，然后在会话的其余时间记住它。

从用户说的第一件事开始捕获 fragments，包括初始提示。

首次写入时，在顶部放置一个带工作标题的 H1（之后可以更改），仅此而已——无元数据、无目录、无日期。

</what-to-do>

<supporting-info>

## 什么是 fragment

Fragment 是任何可能存活到最终文章中的文本片段。它必须_对作者可读_——作者能看出它的意思——但不需要定义其术语或对冷读者可理解。标准是"这是一段好写作吗？"而非"这是自包含的论点吗？"

Fragments 故意异质。以下是 fragment 可能的示例：

- 你想在某处使用但还不知道在哪的犀利句子。
- 带一行理由的断言。
- 小品：发生过的事、代码片段、场景、类比。
- 半想法："关于 X 感觉像 Y 的东西，以后再想清楚。"
- 引用、对话片段、无意中听到的话。
- 凭感觉聚集在一起的相关观察列表。
- 抱怨、坦白、妙语。

小说家的日记是模型：多年的无结构观察后来被挖掘为原材料。Fragments 就是观察。

## 文件格式

```markdown
# 工作标题

第一个 fragment 在这里。

可以是多段。可以包括列表、代码、引用——fragment 自然采取的
任何形状。

---

第二个 fragment。

---

> 用户想保留的引用行。

对它的反应。

---

- 一组相关观察
- 凭感觉聚集在一起
- 想要彼此靠近
```

Fragments 之间用水平线（`\n---\n`）分隔。正文内无标题。无标签。除添加顺序外无排序。

## 写作节奏

安静地追加。不要为每个 fragment 请求许可。顺带提及你添加了什么（"加上那个"），但不要用保存对话框打断对话。

每次写入前：从磁盘重新阅读文件。用户可能在回合之间编辑、重新排序或删除了 fragments——保留他们的更改。永远不要覆盖文件；只追加（或，如果用户要求，就地编辑特定 fragment）。

用户随时可以说"删掉最后一个"、"重写那个更犀利"、"合并那两个"。将这些视为一等指令。

</supporting-info>
