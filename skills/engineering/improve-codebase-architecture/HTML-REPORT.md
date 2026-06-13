# HTML 报告格式

架构审查渲染为操作系统临时目录中的单个自包含 HTML 文件。Tailwind 和 Mermaid 都来自 CDN。Mermaid 可靠地处理图形图表；手工构建的 div 和内联 SVG 处理更具编辑性的视觉效果（质量图、截面图）。混合使用两者 —— 不要什么都依赖 Mermaid，它会开始看起来千篇一律。

## 脚手架

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Architecture review — {{repo name}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
    <style>
      .seam { stroke-dasharray: 4 4; }
      .leak { stroke: #dc2626; }
      .deep { background: linear-gradient(135deg, #0f172a, #1e293b); }
    </style>
  </head>
  <body class="bg-stone-50 text-slate-900 font-sans">
    <main class="max-w-5xl mx-auto px-6 py-12 space-y-12">
      <header>...</header>
      <section id="candidates" class="space-y-10">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## 头部

仓库名称、日期和一个紧凑的图例：实线框 = 模块，虚线 = 接缝，红色箭头 = 泄漏，粗暗框 = 深模块。不需要介绍段落 —— 直接进入候选。

## 候选卡片

图表承担重任。文字简洁、朴素，直接使用术语表中的词汇（[LANGUAGE.md](LANGUAGE.md)），不做铺垫。

每个候选是一个 `<article>`：

- **标题** —— 简短，命名深化（例如"折叠 Order intake 管道"）。
- **徽章行** —— 推荐强度（`Strong` = emerald，`Worth exploring` = amber，`Speculative` = slate），加上依赖类别标签（`in-process`、`local-substitutable`、`ports & adapters`、`mock`）。
- **文件** —— 等宽列表，`font-mono text-sm`。
- **Before / After 图表** —— 核心。两列并排。参见下方模式。
- **问题** —— 一句话。什么在痛。
- **解决方案** —— 一句话。什么改变了。
- **收益** —— 要点，每条 ≤6 个词。例如"测试命中一个接口"、"Pricing 逻辑停止泄漏"、"删除 4 个浅包装器"。
- **ADR 提示**（如适用）—— amber 色调框中的一行。

不需要解释段落。如果图表需要一个段落才能被理解，重新绘制图表。

## 图表模式

选择适合候选的模式。混合使用。不要让每个图表看起来都一样 —— 多样性本身就是目的之一。

### Mermaid 图（依赖/调用流的主力）

当重点是"X 调用 Y 调用 Z，看看这团混乱"时使用 Mermaid `flowchart` 或 `graph`。用 Tailwind 样式的卡片包裹，使其不显得突兀。用 classDef 将泄漏边着色为红色，深模块为暗色。序列图适合"之前：6 次往返；之后：1 次。"

```html
<div class="rounded-lg border border-slate-200 bg-white p-4">
  <pre class="mermaid">
    flowchart LR
      A[OrderHandler] --> B[OrderValidator]
      B --> C[OrderRepo]
      C -.leak.-> D[PricingClient]
      classDef leak stroke:#dc2626,stroke-width:2px;
      class C,D leak
  </pre>
</div>
```

### 手工构建的框线图（当 Mermaid 的布局与你冲突时）

模块作为带边框和标签的 `<div>`。箭头作为内联 SVG `<line>` 或 `<path>` 元素，绝对定位在相对容器上。当你想让"after"图表感觉像一个粗边框的深模块且内部灰显时使用 —— Mermaid 无法以正确的权重渲染。

### 截面图（适合分层的浅度）

堆叠水平条带（`h-12 border-l-4`）以显示调用经过的层。之前：6 个薄层每个什么都不做。之后：1 个厚条带标记合并的职责。

### 质量图（适合"接口与实现一样宽"）

每个模块两个矩形 —— 一个用于接口表面积，一个用于实现。之前：接口矩形几乎与实现矩形一样高（浅）。之后：接口矩形矮，实现矩形高（深）。

### 调用图折叠

之前：函数调用树渲染为嵌套框。之后：同一棵树折叠到一个框中，现在内部的调用显示为淡化。

## 样式指南

- 偏编辑风格，而非企业仪表板。充足的空白。标题可选用衬线体（`font-serif` 与 stone/slate 搭配效果好）。
- 谨慎用色：一个强调色（emerald 或 indigo）加红色表示泄漏、amber 表示警告。
- 图表保持约 320px 高，以便 before/after 舒适地并排显示而不需滚动。
- 图表内的模块标签使用 `text-xs uppercase tracking-wider` —— 它们应读作示意性的，而非 UI。
- 唯一的脚本是 Tailwind CDN 和 Mermaid ESM 导入。报告是静态的 —— 没有应用代码，没有超出 Mermaid 自身渲染的交互。

## 最佳推荐部分

一个更大的卡片。候选名称，一句话说明原因，锚点链接到其卡片。就这些。

## 语调

朴素的英文，简洁 —— 但架构名词和动词直接来自 [LANGUAGE.md](LANGUAGE.md)。简洁不是偏离的借口。

**精确使用：** module、interface、implementation、depth、deep、shallow、seam、adapter、leverage、locality。

**永远不要替换：** component、service、unit（代替 module）· API、signature（代替 interface）· boundary（代替 seam）· layer、wrapper（代替 module，当你指的是 module 时）。

**适合风格的措辞：**

- "Order intake 模块是浅的 —— 接口几乎与实现匹配。"
- "Pricing 跨接缝泄漏。"
- "深化：一个接口，一个测试位置。"
- "两个 adapter 证明接缝合理：生产中 HTTP，测试中内存。"

**收益要点**用术语表中的词汇命名收益：*"局部性：bug 集中在一个模块"*、*"杠杆效应：一个接口，N 个调用点"*、*"接口缩小；实现吸收包装器"*。不要写*"更容易维护"*或*"更干净的代码"* —— 这些术语不在术语表中，不配出现。

不要犹豫、不要清嗓子、不要"值得注意的是……"。如果一句话可以成为要点，就做成要点。如果一个要点可以删减，就删减。如果一个术语不在 [LANGUAGE.md](LANGUAGE.md) 中，在发明新术语之前先找一个在其中的。
