# CONTEXT.md 格式

## 结构

```md
# {上下文名称}

{一两句话描述这个上下文是什么以及为什么存在。}

## 语言

**Order**：
{一两句话描述该术语}
_避免使用_：Purchase, transaction

**Invoice**：
交付后发送给客户的付款请求。
_避免使用_：Bill, payment request

**Customer**：
下订单的个人或组织。
_避免使用_：Client, buyer, account
```

## 规则

- **要有主见。** 当同一概念有多个词时，选择最好的一个，将其余列在 `_避免使用_` 下。
- **保持定义简洁。** 最多一两句话。定义它**是什么**，而非它做什么。
- **只包含特定于此项目上下文的术语。** 通用编程概念（超时、错误类型、工具模式）即使项目大量使用也不属于这里。在添加术语之前问：这是此上下文独有的概念，还是通用编程概念？只有前者属于。
- **当出现自然集群时在子标题下分组**。如果所有术语属于一个凝聚的领域，扁平列表就行。

## 单上下文 vs 多上下文仓库

**单上下文（大多数仓库）：** 仓库根目录一个 `CONTEXT.md`。

**多上下文：** 仓库根目录的 `CONTEXT-MAP.md` 列出各上下文、它们的位置以及如何相互关联：

```md
# Context Map

## 上下文

- [Ordering](./src/ordering/CONTEXT.md) — 接收和跟踪客户订单
- [Billing](./src/billing/CONTEXT.md) — 生成发票并处理付款
- [Fulfillment](./src/fulfillment/CONTEXT.md) — 管理仓库拣货和发货

## 关系

- **Ordering → Fulfillment**：Ordering 发出 `OrderPlaced` 事件；Fulfillment 消费它们开始拣货
- **Fulfillment → Billing**：Fulfillment 发出 `ShipmentDispatched` 事件；Billing 消费它们生成发票
- **Ordering ↔ Billing**：`CustomerId` 和 `Money` 的共享类型
```

该 skill 推断适用哪种结构：

- 如果 `CONTEXT-MAP.md` 存在，读取它来查找上下文
- 如果只有根 `CONTEXT.md` 存在，则为单上下文
- 如果都不存在，在第一个术语确定时按需创建根 `CONTEXT.md`

当多个上下文存在时，推断当前主题与哪个相关。如果不清楚，就询问。
