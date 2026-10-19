---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺乏时间而一直搁置的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成任务后，它会创建草稿 PR，供开发者审查。这样可以把繁琐工作交给智能体，加快开发进程，让开发者专注于更重要的工作。

本课将探索：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确限定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有为代码添加文档。团队希望先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验：在游戏详情页展示同类别的其他游戏，方便用户继续浏览。暂时不需要完整精致的实现，只要能用于 UX 验收测试即可。虽然这项工作目前受到阻碍，但还有优先级更高的议题需要处理。

这些工作都可能被不断延后，很适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者先做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台像开发者一样处理任务。你可以通过多种方式启动工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进展、设置环境并开始工作。你可以在会话进行中或完成后查看进度；准备好审查方案时，Copilot 会在拉取请求中提及你。

## 明确限定指令范围的重要性

GitHub Copilot 不会让你跳过思考。看似简单的操作也可能涉及复杂性，因此要仔细限定[云智能体任务的范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、不断学习和试验，并根据结果调整。生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已经完成相应练习，你可能看过 `.github/copilot-instructions.md`；否则可以现在快速查看。在分配工作前，只读浏览存储库提供的指令文件，以便稍后观察其影响：

- `.github/copilot-instructions.md`：查看 **Code standards**，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将指令限定于 `**/*.test.ts` 文件。

下一节将把 `Code lacks documentation` 议题分配给云智能体。观察 PR 中的 TSDoc 注释、TypeScript 约定和文件注释头；这些要求来自指令文件。稍后的 PR 审查练习还会再次说明这一点。

## 配置 Copilot 云智能体的开发环境

编写代码通常需要特定环境和初始化脚本，Copilot 也一样。云智能体在 GitHub Actions 中运行，可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义环境。此工作流在智能体开始工作前运行，使其可以使用所需的开发工具和依赖项。

本实验已预先配置此工作流：安装 Node.js 和项目依赖项、安装端到端测试所需的浏览器，并为单个 Astro 应用迁移和填充 SQLite 数据库。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub Actions 工作流相似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。等待 Copilot 启动前，可以先验证工作流能否成功运行。

## 添加文档

许多项目的文档已经过时，或完全缺失。这类技术债务常常得不到处理，降低工作效率、增加维护代码库和引入新开发者的难度。Copilot 擅长生成文档，因此很适合将此议题分配给云智能体。它会在后台生成文档，之后可以审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未显示 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> 待办事项可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方法。

13. 查看 Copilot 计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session**。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 查看实时会话，了解 Copilot 如何探索代码、分析项目状态、规划方案并编写代码。

这可能需要几分钟。云智能体的主要优势之一是能够异步处理任务，让你可以同时处理其他工作。接下来将再分配一项任务给云智能体，然后转而开发应用程序功能。

## 在游戏详情页添加相关游戏

云智能体可以让你和它同时处理不同工作。即使为设计团队添加相关游戏不需要很长时间，也可以先将其分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始工作。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二个请求。与软件工程师类似，云智能体无需你持续监控；任务完成后再审查即可。现在可以转而学习如何创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给 Copilot，让它异步处理技术债务、开发功能，或将代码迁移到其他框架。

本课探索了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作。接下来学习创建和使用自定义智能体。云智能体也支持 [MCP 服务器][cloud-agent-mcp]和前面介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
