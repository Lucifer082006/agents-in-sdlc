---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认功能在浏览器中正常运行。本练习将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习模型上下文协议 (MCP) 如何通过服务器为 Copilot 提供新工具，如何在工作区中添加 Playwright MCP 服务器，如何让 Copilot 使用它手动测试筛选功能，以及如何发布分支并创建 PR。

## 什么是模型上下文协议 (MCP)？

智能体模式连接编辑器以外的工具后会更加强大。MCP 是 Copilot 连接外部工具和服务的标准方式。

![智能体模式如何与上下文、LLM 和工具（包括 MCP 服务器及 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体提供与外部工具和服务通信的方式。智能体可以通过资源获取最新信息，也可以使用工具代表你执行操作。MCP 服务器是 AI 智能体与外部工具或服务之间的桥梁，负责管理通信。常见示例包括用于管理 GitHub 存储库、议题和 PR 的 **[GitHub MCP Server](https://github.com/github/github-mcp-server)**，以及提供浏览器自动化能力的 **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**。GitHub 还提供 [MCP 注册表](https://github.com/mcp)，帮助发现和贡献 MCP 服务器。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。其中应已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

模板已包含此配置，因此上一课中的 Copilot 能够读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时让你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，使配置如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数使浏览器在后台运行；Codespace 没有可显示浏览器的桌面环境，因此需要使用此模式。

> [!NOTE]
> Tailspin Toys 已使用 Playwright 进行端到端测试，通常已经安装了浏览器。如果 Copilot 报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并请求确认是否信任。

1. 在 `.vscode/mcp.json` 中找到 `playwright` 项上方的 **Start** 操作并选择它，以立即启动服务器并确认配置正确。
2. 如果 VS Code 要求确认信任服务器，请先审查配置，然后选择信任。

手动启动是可选的；如果跳过，VS Code 会在下一步首次需要其工具时启动服务器并请求信任。

## 测试筛选功能

Playwright MCP 服务器让 Copilot 可以打开页面、导航、应用筛选条件并读取结果。它使用页面的[无障碍树][playwright-mcp-server]，而不是屏幕截图，因此会根据带标签的按钮、链接和列表项等结构化元素进行推理，也能顺便进行简单的无障碍检查。

连接服务器并启动应用后，向 Copilot 发送以下提示：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 打开浏览器、执行每一步并报告结果。将报告与议题中的验收条件对照；如果行为不符合预期，可以追问或要求它修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果提示缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 的更改。
2. 选择 **+** 图标暂存更改，再选择 **Sparkle** 按钮生成提交消息。
3. 选择 **Publish**，将分支推送到存储库。

    ![显示 Copilot 更改的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

### 创建拉取请求

Copilot 可以查找相关议题，并创建与其关联的 PR。

1. 打开 Copilot Chat，选择 **New Chat**。
2. 确保已在智能体下拉菜单中选择 **Agent**。
3. 发送以下提示：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能需要通过 GitHub 身份验证。
5. Copilot 找到正确议题并创建 PR 后，选择 Copilot 返回的链接查看 PR；暂时不要合并。

## 总结与后续步骤

你了解了 MCP 以及 MCP 服务器如何为 Copilot 提供工具，添加了 Playwright MCP 服务器，并在创建 PR 前使用它手动测试筛选功能。MCP 只是扩展 Copilot 的一种方式；下一课将学习如何[创建自定义智能体][next-lesson]，简化无障碍审查等专门工作。

## 资源

- [什么是 MCP？为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
