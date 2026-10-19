---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅知道要做什么，也知道如何完成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本学习路径不会并排比较提示更改前后的效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时展示差异。之后审查云智能体创建的拉取请求时，可以观察指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 遵循一组开发实践规范：数据层始终需要单元测试；UI 应采用深色模式并具有现代感；代码应包含 TSDoc 文档注释；每个文件开头都应有说明文件用途的注释块。通过指令文件，可以让 Copilot 按这些实践开展工作。

### 自定义指令

自定义指令用于向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求，从而生成更符合预期的建议和代码。指令可以规定编码约定、库和注释习惯，也可以应用于整个存储库或特定文件类型。

- `.github/copilot-instructions.md` 是单个指令文件，会随存储库中的**每个**请求发送给 Copilot。文件应包含项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指南，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的指令文件][custom-instructions-support]，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳做法

- 将 `copilot-instructions.md` 用于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明应该和不应该采用的写法。

指令文件没有唯一的编写方式。与 AI 的其他用法一样，可以通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件。示例项目包含适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应产品的课程会在适用时介绍这些入口。
>
> 如需模板或起点，可以浏览包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## 浏览项目中的自定义指令文件

查看此存储库提供的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 指导；这些内容适用于所有 Copilot 交互。
3. 打开 `.github/instructions` 文件夹。查看 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；其中的 glob 相对于存储库根目录，决定适用文件。此处会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对单元测试的具体说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并为 Copilot 提供示例。文件路径相对于指令文件所在目录，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍项目编码规范，但尚未要求代码中包含文档注释。接下来将添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库规范

接下来要做的唯一编辑，是添加项目级规则，要求代码中包含 TSDoc 文档注释和文件级注释头。之后在本学习路径中分配议题时，云智能体会遵循这些规则。

先创建分支：

```bash
git checkout -b custom-instructions-cloud
```

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常在第 27 行附近）。该部分记录了项目编码规范，但目前没有要求代码中包含文档注释。
3. 在现有规范下方添加以下 Markdown：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级 `.github/copilot-instructions.md` 适合放置项目范围内的标准；`*.instructions.md` 则适用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令文件。下一课将从 `main` 分支启动云智能体，因此必须先将指令更改合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后在下一课分配议题时，云智能体便会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里创建分支是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你已了解 Copilot 如何从项目指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级规范。之后可以在云智能体生成的拉取请求中观察该规范的效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
