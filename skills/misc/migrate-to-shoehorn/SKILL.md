---
name: migrate-to-shoehorn
description: Migrate test files from `as` type assertions to @total-typescript/shoehorn. Use when user mentions shoehorn, wants to replace `as` in tests, or needs partial test data.
---

# 迁移到 Shoehorn

## 为什么用 shoehorn？

`shoehorn` 让你在测试中传递部分数据同时保持 TypeScript 满意。它用类型安全的替代方案替换 `as` 断言。

**仅限测试代码。** 永远不要在生产代码中使用 shoehorn。

测试中 `as` 的问题：

- 被训练不要使用它
- 必须手动指定目标类型
- 双 as（`as unknown as Type`）用于故意的错误数据

## 安装

```bash
npm i @total-typescript/shoehorn
```

## 迁移模式

### 大对象只需少量属性

迁移前：

```ts
type Request = {
  body: { id: string };
  headers: Record<string, string>;
  cookies: Record<string, string>;
  // ...还有 20 个属性
};

it("gets user by id", () => {
  // 只关心 body.id 但必须伪造整个 Request
  getUser({
    body: { id: "123" },
    headers: {},
    cookies: {},
    // ...伪造所有 20 个属性
  });
});
```

迁移后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

it("gets user by id", () => {
  getUser(
    fromPartial({
      body: { id: "123" },
    }),
  );
});
```

### `as Type` → `fromPartial()`

迁移前：

```ts
getUser({ body: { id: "123" } } as Request);
```

迁移后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

getUser(fromPartial({ body: { id: "123" } }));
```

### `as unknown as Type` → `fromAny()`

迁移前：

```ts
getUser({ body: { id: 123 } } as unknown as Request); // 故意的错误类型
```

迁移后：

```ts
import { fromAny } from "@total-typescript/shoehorn";

getUser(fromAny({ body: { id: 123 } }));
```

## 何时使用哪个

| 函数            | 使用场景                                         |
| --------------- | ------------------------------------------------ |
| `fromPartial()` | 传递仍通过类型检查的部分数据           |
| `fromAny()`     | 传递故意的错误数据（保留自动补全） |
| `fromExact()`   | 强制完整对象（之后与 fromPartial 交换）    |

## 工作流

1. **收集需求** - 问用户：
   - 哪些测试文件有导致问题的 `as` 断言？
   - 它们是否处理只有部分属性重要的大对象？
   - 它们是否需要传递故意的错误数据用于错误测试？

2. **安装和迁移**：
   - [ ] 安装：`npm i @total-typescript/shoehorn`
   - [ ] 查找含 `as` 断言的测试文件：`grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"`
   - [ ] 将 `as Type` 替换为 `fromPartial()`
   - [ ] 将 `as unknown as Type` 替换为 `fromAny()`
   - [ ] 添加 `@total-typescript/shoehorn` 的导入
   - [ ] 运行类型检查验证
