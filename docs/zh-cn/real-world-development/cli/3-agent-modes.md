---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚为项目添加了一项小功能，但较大的更改需要更完善的流程。GitHub Copilot CLI 可融入组织现有的开发工作流，帮助确保以正确方式构建正确功能。本课将从 GitHub 议题开始，开发新功能、验证代码和行为，并最终成功合并。

> [!NOTE]
> 后续功能工作将继续使用同一对话和分支。通常会为不同文件类型创建不同分支或 PR；本研讨会会简化流程，以便专注于核心概念。

本课将学习如何从 GitHub 议题开始新对话、在 Plan 模式中确定需求、使用 Autopilot 模式实现功能、审查代码并在转发的浏览器中手动验证功能。后续课程还会更新存储库指令、自定义 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录日益增长，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了功能，但组合类别等细节需要在编码前确认。你将使用 Plan 模式确定这些决定，再通过 Autopilot 授权范围明确的实现。

## 背景

在开发流程中引入 AI 编码智能体不会改变基本原则。典型流程如下：

1. 从描述工作内容的已登记议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查通过且持续集成成功后合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但通常会采用上述流程的某种变体。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样接受审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换：

- **Interactive**：你和智能体协作；智能体提出更改并等待你输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直至完成任务。

先使用 Plan 模式审查计划，再使用 Autopilot 实现。

## 从议题开始

1. 返回 Codespace；如果已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中 **Allow users to filter games by category and publisher** 议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

提示中会出现 `#7`（或类似编号）。`#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

对于复杂工作，先花时间规划实现方案很有帮助。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式，确认提示下方显示 **Plan**。
2. 在议题引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的方案。

3. Copilot 可能会询问筛选功能的行为细节。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 查看计划，并要求 Copilot 修订与你的决定不符之处。

## 批准 Autopilot

计划审查完成后，就可以实现了。Autopilot 会持续迭代，直至认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)** 或安装版本中类似的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 根据计划生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告依赖项缺失或端口冲突，请先解决环境问题，再判断检查结果。

## 审查并验证实现

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式，确认当前模式不再是 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，审查筛选实现和测试。
3. 将结果与议题和规划时的决定进行比较。
4. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查结果，并要求 Copilot 修复失败项。

## 探索新功能

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别和发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果行为符合议题要求和计划。
5. 返回 Codespace，并要求 Copilot 停止开发服务器。

## 总结与后续步骤

你已使用不同对话模式构建并审查功能：从 GitHub 议题开始，在 Plan 模式中确定需求，在 Autopilot 模式中实现，并审查代码、在转发浏览器中手动验证功能。

接下来通过[自定义指令][next-lesson]深入了解如何确保生成的代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
