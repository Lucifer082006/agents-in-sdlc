---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码并非开发工作的全部。我们还需要处理数据和外部服务，并让 Copilot 使用更多自动化工具。MCP 服务器可以为 Copilot CLI 提供内置功能以外的工具和服务。

本课将介绍模型上下文协议 (MCP) 及其在 Copilot CLI 中的用法，按需添加 Playwright MCP 服务器，并让智能体操作浏览器探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更改还需要实际与 UI 交互。你希望 Copilot 像用户一样使用网站，以便进一步自动化更改验证过程，并增强对功能行为的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供与外部工具和服务通信的方式。智能体可以实时获取最新信息，并代表你执行操作。MCP 服务器是智能体与外部工具和服务之间的桥梁，每个服务器都提供一组工具和资源。

常见的 MCP 服务器包括用于管理 GitHub 存储库、议题和拉取请求的 **[GitHub MCP Server][github-mcp]**，以及提供浏览器自动化功能的 **[Playwright MCP Server][playwright-mcp-server]**。GitHub 还提供 [MCP 注册表][mcp-registry]，帮助发现服务器并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置 Playwright MCP，请跳过这些设置步骤并继续后续内容。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入服务器名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，再按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能行为。现在使用 Copilot 中的 Playwright MCP 服务器执行相同验证。

1. 发送以下提示，让 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行选择 MCP 服务器，不一定需要明确指出。但提供提示可帮助它更稳定地选择合适工具。

2. 观察 Copilot 如何使用浏览器检查功能。
3. 阅读报告，确认功能行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并提供报告。

## 总结与后续步骤

你已使用 Copilot CLI 中的 Playwright MCP 服务器在真实浏览器中探索功能。接下来将[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [什么是 MCP？为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
