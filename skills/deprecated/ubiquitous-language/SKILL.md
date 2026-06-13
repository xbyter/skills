---
name: 统一语言
description: 从当前对话中提取 DDD 风格的统一语言术语表，标记歧义并提出规范术语建议。保存到 UBIQUITOUS_LANGUAGE.md。当用户想要定义领域术语、构建术语表、巩固术语、创建统一语言，或提到"领域模型"或"DDD"时使用。
disable-model-invocation: true
---

# 统一语言

从当前对话中提取并形式化领域术语为一致的词汇表，保存到本地文件。

## 流程

1. **扫描对话**中的领域相关名词、动词和概念
2. **识别问题**：
   - 同一个词用于不同概念（歧义）
   - 不同的词用于同一概念（同义词）
   - 模糊或过度负载的术语
3. **提出规范词汇表**，带有主观的术语选择
4. **写入工作目录的 `UBIQUITOUS_LANGUAGE.md`**，使用下方格式
5. **在对话中内联输出摘要**

## 输出格式

编写 `UBIQUITOUS_LANGUAGE.md` 文件，结构如下：

```md
# Ubiquitous Language

## Order lifecycle

| Term        | Definition                                              | Aliases to avoid      |
| ----------- | ------------------------------------------------------- | --------------------- |
| **Order**   | A customer's request to purchase one or more items      | Purchase, transaction |
| **Invoice** | A request for payment sent to a customer after delivery | Bill, payment request |

## People

| Term         | Definition                                  | Aliases to avoid       |
| ------------ | ------------------------------------------- | ---------------------- |
| **Customer** | A person or organization that places orders | Client, buyer, account |
| **User**     | An authentication identity in the system    | Login, account         |

## Relationships

- An **Invoice** belongs to exactly one **Customer**
- An **Order** produces one or more **Invoices**

## Example dialogue

> **Dev:** "When a **Customer** places an **Order**, do we create the **Invoice** immediately?"
> **Domain expert:** "No — an **Invoice** is only generated once a **Fulfillment** is confirmed. A single **Order** can produce multiple **Invoices** if items ship in separate **Shipments**."
> **Dev:** "So if a **Shipment** is cancelled before dispatch, no **Invoice** exists for it?"
> **Domain expert:** "Exactly. The **Invoice** lifecycle is tied to the **Fulfillment**, not the **Order**."

## Flagged ambiguities

- "account" was used to mean both **Customer** and **User** — these are distinct concepts: a **Customer** places orders, while a **User** is an authentication identity that may or may not represent a **Customer**.
```

## 规则

- **要有主见。** 当多个词表示同一概念时，选择最好的并将其他列为应避免的别名。
- **显式标记冲突。** 如果术语在对话中被歧义使用，在"Flagged ambiguities"部分中指出并给出清晰建议。
- **只包含领域专家相关的术语。** 跳过模块或类名，除非它们在领域语言中有意义。
- **保持定义紧凑。** 最多一句话。定义它是什么，而非它做什么。
- **展示关系。** 使用粗体术语名称并在明显时表达基数。
- **只包含领域术语。** 跳过通用编程概念（array、function、endpoint），除非它们有领域特定含义。
- **当自然集群出现时将术语分组到多个表格**（例如按子域、生命周期或角色）。每组有自己的标题和表格。如果所有术语属于单一凝聚领域，一个表格即可——不要强制分组。
- **编写示例对话。** 开发者与领域专家之间的简短对话（3-5 次交流），展示术语如何自然交互。对话应澄清相关概念之间的边界并展示术语的精确使用。

<example>

## Example dialogue

> **Dev:** "How do I test the **sync service** without Docker?"

> **Domain expert:** "Provide the **filesystem layer** instead of the **Docker layer**. It implements the same **Sandbox service** interface but uses a local directory as the **sandbox**."

> **Dev:** "So **sync-in** still creates a **bundle** and unpacks it?"

> **Domain expert:** "Exactly. The **sync service** doesn't know which layer it's talking to. It calls `exec` and `copyIn` — the **filesystem layer** just runs those as local shell commands."

</example>

## 重新运行

在同一对话中再次调用时：

1. 读取现有的 `UBIQUITOUS_LANGUAGE.md`
2. 融入后续讨论中的新术语
3. 如果理解已演变则更新定义
4. 重新标记任何新的歧义
5. 重写示例对话以融入新术语
