---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。通过[指令文件][instruction-files]，可以向 Copilot 提供背景信息和代码生成指导。

本练习将探索项目上下文、编码指南和文档规范如何通过存储库自定义指令及路径级指令文件传递给 Copilot；使用现有指令生成筛选功能的数据层片段；向 `.github/copilot-instructions.md` 添加存储库级规范；再次运行相同提示并观察代码变化；最后提交指令更新和筛选功能基础。

> [!CAUTION]
> 生成的代码可能不完全符合指定标准，因为 Copilot 具有非确定性。本练习关注更新指令后行为的变化趋势，而不是要求输出逐字符一致。

## 指令文件

### 场景

Tailspin Toys 的开发规范包括：数据层始终需要单元测试；UI 使用深色模式并保持现代感；代码使用 TSDoc 文档注释；每个文件开头都应有说明其用途的注释块。

### 自定义指令

自定义指令可提供编码风格、要求、偏好的库和注释习惯，帮助 Copilot 生成更符合项目规范的建议。

- `.github/copilot-instructions.md` 会随存储库中的**每个**请求发送给 Copilot，适用于技术栈、项目概述、最佳实践等全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指南，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级指令文件及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 也支持 AGENTS.md 和 CLAUDE.md 等其他标准的指令文件。

### 管理指令文件的最佳做法

- 将项目结构、项目说明和全局标准放在 `copilot-instructions.md` 中。
- 使用 `*.instructions.md` 编写适用于文件类型或具体任务的规则。
- 使用自然语言，确保指导清晰，并提供代码示例。

> [!TIP]
> 项目指令文件可以涵盖 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务。Copilot 也能帮助生成指令文件，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions** 或 Copilot CLI 中的 `/init`。还可以浏览 [awesome-copilot][awesome-copilot] 获取模板和示例。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## 浏览项目中的自定义指令文件

1. 打开 `.github/copilot-instructions.md`，查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分，并留意 **GitHub Actions Workflows** 指导。
2. 浏览 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等指令。
3. 打开 `.github/instructions/unit-tests.instructions.md`，查看 `applyTo` 字段如何通过相对于存储库根目录的 glob 匹配 TypeScript 测试文件（例如 `**/*.test.ts`）。
4. 查看 `.github/instructions/drizzle.instructions.md` 的末尾，了解如何链接其他指令文件和项目示例。此处的路径相对于该指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 的 **Code formatting requirements** 部分还没有要求代码中包含文档注释。接下来将添加 TSDoc 注释和文件注释头规则。

## 创建分支

1. 返回 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；相应步骤会说明需要选择的设置。

1. 关闭之前练习中的编辑器标签页。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道帮助程序应位于何处。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常涉及帮助程序文件和测试。生成的帮助程序会遵循 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定，但还不会添加 TSDoc 注释或文件注释头。

注意，生成的帮助程序是一个类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。

> [!CAUTION]
> Copilot 具有非确定性，因此也可能自行添加文档注释。即使发生这种情况，更新指令后提高一致性仍是本练习的重点。

## 添加新的存储库规范

1. 重新打开 `.github/copilot-instructions.md`，找到 **Code formatting requirements**。
2. 在现有规范下方添加以下 Markdown：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. 保存文件。

> [!TIP]
> 存储库级 `.github/copilot-instructions.md` 适用于全局指导；`*.instructions.md` 则适用于特定语言、文件类型或任务。文档注释规则属于项目级标准。

## 再次运行提示并观察变化

1. 在 Copilot Chat 中选择 **New Chat**，清除对话内容。
2. 选择 `src/lib/publishers.ts`，再次发送上面的相同提示。
3. 查看提议的文件。文件开头现在应有类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

4. 查看提议的函数，确认它包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于后续练习要使用的代码。

## 提交指令并推送分支

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行 `git status`，确认指令和帮助程序更改都已存在。
3. 暂存并提交指令更改，以及 Copilot 为帮助程序基础所做的全部更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   git push -u origin custom-instructions
   ```

如果 `git status` 显示了其他辅助更改（例如测试），也要将这些文件加入提交。

## 创建并合并拉取请求

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确认已选择 **Agent**。
3. 发送以下提示，让 Copilot 查找与更新指令文件相关的议题并创建 PR：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。提示允许它代表你执行 GitHub 操作时，先审查命令，再按需选择 **Allow**。
5. PR 创建后，发送以下提示，将 PR 合并到 `main`，并在本地切换回 `main`、拉取最新代码：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. 提示允许 Copilot 执行 GitHub 操作和 shell 命令时，先审查命令，再按需选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结与后续步骤

你了解了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中：

- 发送代码生成提示，观察现有指令的影响。
- 在 `.github/copilot-instructions.md` 中添加存储库级标准。
- 再次发送相同提示，观察建议代码如何遵循新标准。
- 将指令和帮助程序基础提交到 `main`，以供下一课使用。

接下来，Copilot 将在[智能体模式][next-lesson]中应用这些指令，跨代码库添加功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
