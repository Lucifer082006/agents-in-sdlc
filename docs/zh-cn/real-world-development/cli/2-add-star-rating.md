---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并通过第一个拉取请求合并代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在开始对项目进行首次更改。游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。让智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将学习如何在功能分支中开始专注的 Copilot 对话、请求小范围的项目更改、使用 `/diff` 审查更改、在 Codespaces 转发的浏览器中运行应用确认效果，以及创建并合并第一个拉取请求。

## 场景

Tailspin Toys 的每个游戏都有星级评分，详情页已显示评分，但主页游戏卡片只显示标题、类别、发行商和说明。作为热身练习，让智能体在每张卡片上显示现有评分。这是适合首次使用 Copilot CLI 的小型独立更改。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可供之后继续，但文件和分支仍是磁盘上的普通 Git 状态。

对话中会显示你的提示及智能体回复、智能体探索和编辑文件时的工具活动，以及可使用 `/diff` 检查的更改。

## 开始对话并请求更改

1. 返回 Codespace。
2. 如终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请执行：

   ```bash
   copilot --yolo
   ```

4. 输入 `/new` 并按 <kbd>Enter</kbd>，开始新会话。
5. 发送以下提示：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、找到显示游戏详情的文件并创建必要代码。你已使用 Copilot CLI 添加了新功能。

## 审查差异

即使是小型更改，在合并前也应审查所有 AI 生成的更改。

1. 输入 `/diff` 并检查每个更改文件。
2. 确认游戏卡片在评分存在时显示数值，`starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 检查 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性，因此实际代码可能有所不同。应审查行为，而不是要求实现完全一致。

## 检查更改

不要只阅读代码并假设它可以运行。让 Copilot 启动网站，并在 Codespaces 转发的浏览器中检查 UI。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。创建 PR 时，Copilot CLI 会推送现有提交并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Copilot CLI 显示的 URL 打开 PR。
4. 审查更改文件和检查结果。
5. 准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

你已使用 GitHub Copilot CLI 发布第一个更改：在功能分支中开始专注的对话，让智能体修改游戏卡片，使用 `/diff` 审查，运行应用并在转发的浏览器中确认评分，最后创建并合并第一个 PR。

接下来从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
