---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本学习路径中，你让 GitHub Copilot 完成了几项改善用户体验和增加功能的工作：为代码添加文档、创建相关游戏功能，以及实现高对比度和浅色模式。现在审查代码更改，并在需要时向 Copilot 提供反馈，然后再让团队其他成员开始审查。

### 场景

使用生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终要审查生成的代码，并遵循正常的 DevOps 流程。现在来审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提供的建议。

## GitHub Copilot 云智能体安全性

云智能体会异步且自主地执行任务，因此设置了一些安全限制：

- Copilot 只能读取存储库，并且只能写入用于完成任务的分支。
- 云智能体在 GitHub Actions 中运行，并使用单独的临时环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先通过 GitHub.com 的标准 PR 界面查看 Copilot 云智能体创建的第一个拉取请求，即为代码添加文档的 PR。

> [!NOTE]
> PR 中可能会显示 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 访问外部资源（包括外部 MCP 服务器）的权限有限。你也可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库，选择 **Pull Requests**。
2. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。请等待任务完成，这可能需要几分钟。

3. 选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

4. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能有所不同。留意 TSDoc 注释、TypeScript 约定和文件注释头，它们来自前面查看的自定义指令文件。
5. 确认更改后，返回 **Conversation** 选项卡并向下滚动。
6. 如果工作流等待批准，请选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. 在 PR 的检查部分确认工作流已排队；检查应会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

你可以像联系团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会看到评论并启动新会话来处理反馈。由于结果具有非确定性，这里不提供固定提示。可以要求它在代码文件顶部添加用途说明注释头、在 TypeScript 和 Astro 文件中添加 TSDoc 注释，或为 Astro 应用结构创建 README。

1. 添加评论并提及 **@copilot**，请求它更新生成的文档。
2. 选择 **View Session**，查看 Copilot 如何启动新会话并进行更新。
3. 可以选择 **Back to pull request** 返回 PR。
4. Copilot 完成更新后，PR 中会出现新提交。选择 **Files changed** 审查更改。

可以继续迭代，直至满意。随后将 PR 标记为可供审查，并合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

1. 返回 github.com 上的存储库，选择 **Pull Requests**。
2. 打开标题类似 **Show related games on the game details page** 的 PR，并在 **Files changed** 选项卡中审查代码。
3. 确认更改后，返回 **Conversation** 选项卡。如果有工作流等待批准，选择 **Approve and run workflows**。
4. 等待 PR 检查完成。可选：在 Codespace 中切换到 Copilot 创建的分支（将 `<branch-name>` 替换为实际分支名，例如 **copilot/fix-8**）并手动测试：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已创建相关游戏功能。你可以通过在 **Conversation** 选项卡中评论来继续迭代，例如调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释。请注意，这项工作是在更新自定义指令之前分配的。

## 审查无障碍功能

最后审查由无障碍自定义智能体实现的功能。此 PR 应包含第 3 课要求的高对比度模式，以及第 4 课在 mission control 中追加的浅色模式。

1. 在 github.com 上打开存储库的 **Pull Requests**，选择标题类似 **Add high contrast mode to website** 的 PR。
2. 如果 Copilot 仍在工作，请等待 PR 不再显示 **[WIP]**。
3. 选择 **Files changed**，重点查看模式切换 UI、用于保存偏好的本地存储、样式更改、无障碍属性（ARIA 标签和键盘导航），以及模式切换代码。
4. 返回 **Conversation** 选项卡。如果有工作流等待批准，选择 **Approve and run workflows**，然后等待 PR 检查完成。
5. 可选：在 Codespace 中检出 Copilot 创建的分支并启动应用，手动测试高对比度和浅色模式，确认设置可跨页面重新加载保存：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

自定义无障碍智能体帮助 Copilot 遵循无障碍最佳实践。如果发现问题，可以像前面一样在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体适合处理真实待办事项，让你能同时处理其他工作。可以创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表实现分页。
- 为数据访问帮助程序添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径，并：

- **查看了存储库自带的自定义指令文件**，了解它们如何影响云智能体生成的内容。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划并异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式。
- **使用 Copilot 智能体页面作为 mission control**，监控并引导无障碍会话。
- **审查并迭代云智能体的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾与后续步骤

云智能体学习路径已完成。其他学习路径可以帮助你继续探索：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中使用 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以尝试将重构或测试覆盖率议题分配给云智能体，为其他领域创建自定义智能体，或为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
