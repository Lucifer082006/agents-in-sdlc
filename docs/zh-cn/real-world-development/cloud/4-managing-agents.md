---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你让 Copilot 云智能体处理了三项改善用户体验和增加功能的任务。云智能体虽然可以自主异步工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在此 mission control 中，可以查看所有有未关闭 PR 的智能体任务、了解智能体采取的操作，还可以引导正在进行的会话。

本课将学习：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话以请求其他功能。

## 场景

团队在分配高对比度模式任务后，意识到还应增加浅色模式。网站样式和切换功能已经在更新，因此他们希望引导智能体同时实现两种模式。

## 查看 Copilot 云智能体任务

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务。你应能看到分配给 Copilot 的任务，例如为代码库补充文档、生成用于修改游戏的 API，以及为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 执行的操作，例如检出代码、创建工作环境、设置 MCP 服务器和完成任务的其他步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 工作状态和执行方案而异。

4. 查看右侧的 PR 面板，监控 PR 和文件更改。

## 引导云智能体

现在要求 Copilot 在实现高对比度模式时，也加入浅色模式切换。

1. 选择添加高对比度模式的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到 Copilot 完成设置并开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务，突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 观察 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会在完成发送提示时正在执行的步骤后，将新请求纳入工作流程。这需要几分钟，可以借此回顾已学习的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有进行中和已完成的任务、审查智能体工作，还可以调整正在运行的任务范围或提供更多指导。

本课探索了以下概念：

- 使用 mission control 和智能体页面监控云智能体任务。
- 引导正在进行的会话以请求其他功能。

Copilot 正在完成无障碍功能。接下来学习如何[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
