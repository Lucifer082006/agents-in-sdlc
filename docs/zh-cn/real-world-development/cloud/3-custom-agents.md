---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成相应工作。团队可以将专业知识整理为可复用的智能体，例如检查 [WCAG][wcag] 合规性的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

项目的 `.github/agents` 文件夹或全局 `~/.copilot/agents` 目录中的 Markdown 文件都可以定义自定义智能体。每个文件至少需要包含 `name` 和 `description` YAML frontmatter，后续 Markdown 提示则定义智能体的行为、专业知识和指令。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处：两者主要通过 Markdown 文件定义，并指导 AI 执行操作。可以这样区分：**自定义智能体是执行工作的人员，技能是工具**。

自定义智能体拥有独立上下文窗口，可以在工作中编排技能，甚至其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站，也可以调用 PR 工作流或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

本课将学习：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好影响。最近有用户反馈，网站当前的深色主题对比度不足，难以阅读。设计团队希望增加可切换的高对比度模式。

由于无障碍至关重要，团队希望尽快实现此功能，因此将使用自定义智能体生成实现。

## 审查无障碍自定义智能体

项目已为无障碍工作准备好自定义智能体。返回 Codespace 并查看其配置：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览文件中的其他部分，了解其无障碍网站编码职责、最佳实践、HTML、CSS 和 JavaScript 代码示例，以及常见问题。

## 创建并分配议题

Mission control 是管理环境中所有智能体的中心位置。在这里可以向 Copilot 云智能体分配任务、监控任务，并提供额外指导。现在将实现高对比度模式的任务分配给 Copilot：

1. 打开存储库，选择议题选项卡，然后选择 **New issue** 和 **Blank issue**。
2. 将标题设为 `Add high contrast mode to website`。
3. 将说明设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

4. 选择 **Create** 创建议题。
5. 在右侧选择 **Assign to Copilot**，然后从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面，其中高亮显示了自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

6. 选择 **Assign**。Copilot 将在后台开始处理任务。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 自定义智能体][custom-agents]。团队可以将专业知识和标准整理为可复用的智能体，指导 Copilot 更有效地完成特定任务。

本课探索了以下概念：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，从 mission control [监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
