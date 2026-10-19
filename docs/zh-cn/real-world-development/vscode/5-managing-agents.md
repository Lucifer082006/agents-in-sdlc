---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

Copilot 在智能体模式下会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作实时发生在本地计算机上，VS Code 中的 Copilot Chat 会显示工具调用和文件编辑进度。你可以查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，无需离开编辑器。

本练习将学习如何实时监控无障碍智能体的工作、审查并接受文件编辑，以及在会话中途引导智能体添加浅色模式切换。

## 场景

上一课中使用的无障碍自定义智能体正在实现高对比度模式。设计团队又提出一项请求：为觉得默认主题过暗的用户增加浅色模式。无需重新开始对话，只需引导现有会话扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体的每个操作，包括探索的文件、运行的命令和提议的编辑。文件编辑会在聊天中以内联差异的形式显示，并带有 **Keep** 和 **Undo** 控件。

1. 返回上一课中的 Copilot Chat 视图。
2. 滚动聊天内容，查看智能体的工具调用，包括文件读取、编辑和终端命令。
3. 查看内联差异，选择 **Keep** 接受更改，或选择 **Undo** 撤销更改并让智能体重新尝试。
4. 选择聊天顶部的 **Files changed** 指示器，查看本次会话中的全部更改。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式的实现，并查看最终更改摘要。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式。智能体会保留之前工作的上下文，包括修改过的组件、样式方式和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查已有更改并提出浅色模式的实现。
3. 像处理高对比度更改一样，逐项选择 **Keep** 或 **Undo**。
4. 如果智能体偏离预期，可选择输入框旁的 **Stop** 按钮停止，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以要求简化 CSS（使用 CSS 变量而不是重复选择器）、调整方向（将浅色模式整合到现有设置面板，而非增加第三个开关），或扩大范围（为两个开关添加键盘快捷键）。

## 审查工作集

提交前快速检查智能体修改的所有内容：

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有更改的文件，包括 Astro 组件、样式和相关测试。
3. 打开部分文件查看差异，确认包含无障碍模式要求的 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并引导了正在进行的会话：审查文件编辑、接受或拒绝更改，并在保留上下文的同时通过后续提示扩展工作。

本课探索了实时监控智能体工作、接受或拒绝单项编辑，以及在会话中途发送后续提示。下一课将把无障碍工作发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
