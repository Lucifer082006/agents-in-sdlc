---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建结合议题需求、quality-checks 技能和 Playwright MCP 的 QA 自定义智能体。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在创建自定义智能体，整合这些能力并定义清晰的 QA 流程。

本课将学习自定义智能体如何使用指令、技能和 MCP 工具，创建并检查可复用的质量保证配置文件，以及选择 QA 智能体并依据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

已使用的自定义功能各有用途：存储库指令描述团队标准；`quality-checks` 技能打包可重复检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力检查需求并报告发现。它们不会相互替代，也不要求另开对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 提取了预期 QA 工作流和边界，以免它在你只想验证功能时修改功能实现。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 是必需字段；`name` 可选，但有助于显示清晰的名称。
3. 检查指令是否要求 QA 从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改实现代码前会先询问，并且不会提交更改或创建 PR。
5. 如果缺少任何职责或边界，请在继续前让默认智能体修订配置文件。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选对话并选择 QA，让它使用已有的议题和规划决定。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建智能体，它可能不会显示在可用列表中，但上述命令仍会启用它。

2. 发送提示，让 QA 审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成工作并阅读报告。

## 总结与后续步骤

你已添加可复用的专业角色并审查其工作。本课探索了自定义智能体如何与指令、技能和 MCP 工具协作，创建并检查 QA 配置文件，以及选择 QA 智能体审查筛选议题。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告均已准备好审查。接下来将它们整合到[功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
