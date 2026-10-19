---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识。团队可以将专业知识整理为可复用智能体，例如检查 [WCAG][wcag] 合规性的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体可以在项目的 `.github/agents` 文件夹或全局 `~/.copilot/agents` 目录中定义。每个文件都包含至少带有 `name` 和 `description` 的 YAML frontmatter，以及定义其行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]都通过 Markdown 文件指导 AI 执行操作。可以将自定义智能体视为工作者，将技能视为工具。自定义智能体拥有自己的上下文窗口，可以在工作中编排技能，甚至其他智能体。本实验中的无障碍智能体会根据无障碍指南审查并更新网站，也可以调用 PR 工作流或测试管理技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台。用户反馈指出，当前深色主题的文本和背景对比度不足，导致难以阅读。设计团队希望添加可切换的高对比度模式。本练习将使用自定义智能体实现此功能。

本练习将学习自定义智能体的定义方式，以及如何在 Copilot Chat 中使用智能体实现高对比度模式。

## 审查无障碍自定义智能体

项目已准备好无障碍自定义智能体。返回 Codespace 并查看 `.github/agents/accessibility.md`：

1. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。
2. 浏览智能体的职责、无障碍最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 并选择 **New Chat**，开始干净的对话。模式和模型选择因练习而异；每一步会说明相关设置。

1. 如果尚未选择，请在 Chat 视图的智能体下拉菜单中选择 **Agent**。
2. 选择底部的智能体下拉菜单（默认显示当前智能体 **default**）。
3. 从列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会探索代码库、提出更改并应用到项目文件。每项编辑都会在聊天中显示文件路径和差异。
6. 查看 **Files changed** 指示器，了解智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一课将审查并引导正在进行的工作。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 自定义智能体][custom-agents]，即针对特定任务或领域定制的 AI 助手。团队可以将专业知识和标准整理为可复用智能体，指导 Copilot 更有效地完成工作。

本课探索了自定义智能体的定义方式，以及如何在 Copilot Chat 智能体模式中使用它。接下来将[监控并引导智能体的工作][next-lesson]，在同一会话中审查更改，并添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
