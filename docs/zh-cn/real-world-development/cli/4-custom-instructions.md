---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。要让 Copilot 按特定方式完成任务，就需要向它提供相应指导。[指令文件][instruction-files]不仅描述希望生成什么代码，也说明代码应如何组织。现在筛选功能已经实现，接下来将查看 Copilot 使用的指令、添加文档规范并将其应用到代码。

本课将学习存储库指令和按路径限定的指令文件如何提供给智能体，如何更新指令以确保遵循编码标准，以及指令文件如何影响代码。

## 场景

Tailspin Toys 的开发规范包括：

- 注释应说明意图和不明显的决定，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 文档注释，说明用途、参数和返回值，并在适用时说明可注入的 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持同步。
- 保留已有格式和 lint 指导。

## 指令文件

自定义指令可以向 Copilot 提供编码风格、要求、偏好库和注释习惯。

- `.github/copilot-instructions.md` 会随存储库中的**每个**请求发送给 Copilot，包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。使用特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 浏览项目中的自定义指令文件

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`，查看项目说明和编码指导。这些指令适用于此存储库中的每次 Copilot 交互。
3. 浏览 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段及其文件匹配模式。
5. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他指令和项目文件的链接。拆分指令可便于复用，并为 Copilot 提供示例。

## 更新指令文件以符合团队指导

现有文件已经提供了良好起点，但还需要补充新 TypeScript 代码的 TSDoc 注释规范。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（通常在第 35 行附近）。
2. 在最后一条下方添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存指令后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍打开，输入 `/exit`。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复保存的会话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话并确认当前检出的是 `game-filters-cli` 分支。

3. 要求 Copilot 应用新指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。确认新增的 TSDoc 注释准确说明代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何获取指令文件中的上下文，并将新标准应用于筛选功能。接下来将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
