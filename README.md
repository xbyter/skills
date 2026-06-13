<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 面向真正工程师的 Skills

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天用于做真正工程工作的 agent skills —— 不是 vibe coding。

开发真正的应用很难。GSD、BMAD 和 Spec-Kit 等方法试图通过掌控流程来提供帮助。但在此过程中，它们夺走了你的控制权，并使流程中的 bug 难以解决。

这些 skills 被设计为小巧、易于适配且可组合。它们适用于任何模型，基于数十年的工程经验。随意修改它们，让它们成为你自己的。享受吧。

如果你想跟踪这些 skills 的更新以及我创建的新 skills，可以加入我的 newsletter，与约 60,000 名开发者一起：

[订阅 Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速开始（30 秒设置）

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你想要的 skills，以及你想安装到哪些 coding agents 上。**确保选择 `/setup-matt-pocock-skills`**。

3. 在你的 agent 中运行 `/setup-matt-pocock-skills`。它会：
   - 询问你要使用哪个 issue tracker（GitHub、Linear 或本地文件）
   - 询问你在 triage 时给 ticket 打什么标签（`/triage` 使用标签）
   - 询问你想把创建的文档保存在哪里

4. 搞定 —— 你可以开始使用了。

## 为什么要有这些 Skills

我创建这些 skills 是为了解决我在 Claude Code、Codex 和其他 coding agents 中常见的失败模式。

### #1：Agent 没有按我的意愿行事

> "没有人确切知道自己想要什么"
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**。软件开发中最常见的失败模式是需求不对齐。你以为开发者知道你想要什么。然后你看到他们构建的东西 —— 才意识到它根本没有理解你。

在 AI 时代也是一样。你和 agent 之间存在沟通鸿沟。解决方法是 **grilling session（拷问会话）** —— 让 agent 就你正在构建的东西向你提出详细问题。

**解决方法**是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) - 用于非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) - 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 相同，但增加了更多好东西（见下文）

这些是我最受欢迎的 skills。它们帮助你在开始之前与 agent 对齐，并深入思考你要做的变更。**每次**想做变更时都使用它们。

### #2：Agent 太啰嗦了

> 有了 ubiquitous language，开发者之间的对话和代码的表达都源自同一个领域模型。
>
> Eric Evans，[Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题**：在项目开始时，开发者和他们为之构建软件的人（领域专家）通常说着不同的语言。

我在自己的 agents 中也感受到了同样的张力。Agents 通常被扔进一个项目，然后被要求在过程中自行搞懂术语。所以它们用 20 个词来表达 1 个词就够的意思。

**解决方法**是共享语言。这是一份帮助 agents 解码项目中使用的术语的文档。

<details>
<summary>
示例
</summary>

这是一个来自我的 `course-video-manager` 仓库的 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。哪个更容易阅读？

- **之前**："当课程中某个章节内的课时被'真实化'（即在文件系统中获得一个位置）时，会出现问题"
- **之后**："materialization cascade 存在问题"

这种简洁性在一次又一次的会话中带来了回报。

</details>

这内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。它是一个 grilling session，帮助你与 AI 建立共享语言，并在 ADR 中记录难以解释的决策。

很难描述这有多强大。这可能是这个仓库中最酷的技术。试试看，你会明白的。

> [!TIP]
> 共享语言除了减少冗长外还有许多其他好处：
>
> - **变量、函数和文件命名一致**，使用共享语言
> - 因此，**代码库对 agent 来说更容易导航**
> - Agent **花在思考上的 token 也更少**，因为它可以使用更简洁的语言

### #3：代码不能正常工作

> "始终采取小而谨慎的步骤。反馈的频率是你的速度限制。永远不要承担太大的任务。"
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**：假设你和 agent 在要构建什么上已经对齐了。当 agent **仍然**产出垃圾时怎么办？

是时候看看你的反馈循环了。如果没有关于它产生的代码实际如何运行的反馈，agent 就是在盲飞。

**解决方法**：你需要通常的一组反馈循环：静态类型、浏览器访问和自动化测试。

对于自动化测试，red-green-refactor 循环至关重要。即 agent 先写一个失败的测试，然后修复测试。这有助于给 agent 一致的反馈水平，从而产生更好的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) skill**，你可以将其插入任何项目。它鼓励 red-green-refactor 并为 agent 提供关于好测试和坏测试的充分指导。

对于调试，我还构建了一个 **[`/diagnose`](./skills/engineering/diagnose/SKILL.md)** skill，将最佳调试实践封装在一个简单的循环中。

### #4：我们造了一坨泥

> "_每天_都要投资系统的设计。"
>
> Kent Beck，[Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "最好的模块是深的。它们允许通过简单的接口访问大量功能。"
>
> John Ousterhout，[A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题**：大多数用 agents 构建的应用复杂且难以更改。因为 agents 可以极大加速编码，它们也加速了软件熵。代码库以前所未有的速度变得更加复杂。

**解决方法**是 AI 驱动开发的全新方法：关注代码的设计。

这内置于这些 skills 的每个层面：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前询问你涉及哪些模块
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.md) 让 agent 在整个系统的上下文中解释代码

关键的是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 帮助你拯救已经变成一坨泥的代码库。我建议每隔几天在你的代码库上运行一次。

### 总结

软件工程基础比以往任何时候都更重要。这些 skills 是我将这些基础浓缩为可重复实践的最佳努力，帮助你交付你职业生涯中最好的应用。享受吧。

## 参考

### Engineering

我每天用于代码工作的 skills。

- **[diagnose](./skills/engineering/diagnose/SKILL.md)** — 针对棘手 bug 和性能退化的纪律化诊断循环：复现 → 最小化 → 假设 → 插桩 → 修复 → 回归测试。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — Grilling session，根据现有领域模型挑战你的计划，磨练术语，并内联更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)** — 通过 triage role 状态机对 issues 进行分诊。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 在代码库中寻找深化机会，参考 `CONTEXT.md` 中的领域语言和 `docs/adr/` 中的决策。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 搭建按仓库的配置（issue tracker、triage 标签词汇、领域文档布局），供其他 engineering skills 消费。
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 使用 red-green-refactor 循环的测试驱动开发。一次一个垂直切片地构建功能或修复 bug。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 使用垂直切片将任何计划、规格或 PRD 拆分为可独立领取的 GitHub issues。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 将当前对话上下文转化为 PRD 并提交为 GitHub issue。无需访谈 —— 仅综合你已经讨论过的内容。
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)** — 让 agent 放大视角，对不熟悉的代码段给出更广泛的上下文或更高层次的视角。
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一次性原型来完善设计 —— 要么是可运行的终端应用用于状态/业务逻辑问题，要么是从一个路由可切换的多种截然不同的 UI 变体。

### Productivity

通用工作流工具，不限于代码。

- **[caveman](./skills/productivity/caveman/SKILL.md)** — 超压缩通信模式。通过去掉废话将 token 使用量减少约 75%，同时保持完整的技术准确性。
- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 就计划或设计进行无情的面试，直到决策树的每个分支都得到解决。
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 将当前对话压缩为交接文档，以便另一个 agent 可以继续工作。
- **[teach](./skills/productivity/teach/SKILL.md)** — 跨多个会话教授用户新技能或概念，使用当前目录作为有状态的教学工作区。
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)** — 创建具有正确结构、渐进式展示和捆绑资源的新 skills。

### Misc

我保留但很少使用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code hooks 以在执行前阻止危险的 git 命令（push、reset --hard、clean 等）。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建包含章节、题目、解答和说明的练习目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 使用 lint-staged、Prettier、类型检查和测试设置 Husky pre-commit hooks。
