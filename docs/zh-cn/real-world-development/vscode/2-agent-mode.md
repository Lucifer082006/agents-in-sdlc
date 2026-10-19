---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到相关文件、进行更改、运行测试、调试并确定是否有文件遗漏。Copilot 智能体模式可以帮助完成这些工作。

智能体模式可在 IDE 中更自主地工作，先探索项目结构，再进行必要的更新、运行测试等任务，并自动修复发现的问题。接下来使用它为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，支持快速反馈和交互；云智能体则异步工作，像团队成员一样通过议题和拉取请求与你协作。

本练习将学习如何让 Copilot 智能体模式探索项目、识别相关文件、跨 UI 和数据层实现功能，并在合并前审查生成的更改和测试。

## 场景

随着游戏列表增长，你希望用户可以按类别和发行商筛选游戏。上一课已添加发行商帮助程序；现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

Tailspin Toys 是为开发者主题桌游众筹的平台。网站是单个 Astro 应用，在构建时将页面渲染为静态 HTML；页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立后端 API 或客户端 UI 框架。可复用的数据访问帮助程序位于 `src/lib/`，交互功能通过使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

1. 返回上一课结束后所在的 `main` 分支。
2. 在 Codespace 中打开终端并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

3. 运行网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后会显示本地 URL，例如 `http://localhost:4321`。

> [!NOTE]
> 如果出现提示，询问是否要打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭。

4. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Astro 服务器地址。

Codespace 会将本地 URL 自动转发到 `https://<your-codespace-name>-4321.app.github.dev/`，以私有隧道访问开发服务器。

### 浏览网站

- **主页**：显示桌游列表及其标题、图像和说明。
- **游戏详情页**：选择游戏后可查看其标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 并选择 **New Chat**，开始干净的会话。模式和模型选择因练习而异；每一步会说明相关设置。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。
2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本课程使用 Claude Sonnet 4.5 是为了让建议相对一致，并不代表作者偏好某个模型。由于 LLM 具有概率性，建议可能与示例不同，这是正常现象。

> [!NOTE]
> Copilot 可能会调用其他 MCP 命令，但仍应能完成任务。

3. 发送以下提示，查看存储库待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令，然后审查 Copilot 生成的列表和优先级。

## 审查指令文件

开始生成代码前，先查看用于提供项目上下文的 `.github/instructions/ui.instructions.md`。其中包含网站功能开发指导。

1. 在 Codespace 中打开该文件。
2. 查看整体架构、组件设计、可测试性和无障碍原则，以及指向 Astro、Tailwind CSS 和 Drizzle 指令文件的链接。

> [!TIP]
> 指令文件可以链接其他指令文件和项目文件。路径相对于指令文件所在位置，可用于复用和拆分较大的指令集，并为 Copilot 提供示例。

## 实现筛选功能

筛选功能至少需要更新数据层 (`src/lib/`)、添加或更新测试，并为游戏列表页添加筛选 UI。在合并前必须运行并通过测试。

1. 继续当前 Copilot 对话，或选择 **New Chat**。
2. 选择 **Add Context**、**Instructions** 和 **ui**。
3. 确保已选中 **Agent** 和 **Claude Sonnet 4.5**。
4. 发送提示：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. 观察 Copilot 如何探索项目、查找数据层、UI 和测试文件，修改代码并运行测试。按提示选择 **Continue** 运行测试。测试过程中可能会有暂停或失败，Copilot 会继续迭代，直至完成任务且未发现错误。
6. 检查生成的代码，然后回到浏览器探索筛选功能。
7. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

## 总结与后续步骤

本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了智能体模式如何探索项目、识别相关文件、协同修改 UI 和数据层，并在合并前审查更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 延伸练习 — 实现分页

随着游戏列表增长，可能需要分页。可以使用本练习学到的技能提示 Copilot 实现分页，并考虑是否允许用户选择每页数量、如何遵循现有最佳实践，以及是否需要多轮迭代。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
