---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在一套连续的 Tailspin Toys 工作流中使用 GitHub Copilot CLI。你完成了以下工作：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，定制现有 `quality-checks` 技能，并运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能结果和浏览器验证证据。
- 审查完整筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付内容

本研讨会包含两个 PR 阶段，且每个阶段都基于更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片中显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为简化研讨会，我们将这些工作合并在一个 PR 中。

## 不同类型的验证

你通过自动测试、浏览器手动检查和 Copilot 使用 MCP 进行浏览器探索来检查代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会结合这些结果、需求审查和测试覆盖率进行评估。

新测试应弥补实际覆盖缺口；QA 判断无需添加测试也可能是正确的。授权合并前应审查代码和证据，并在更改后更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。本研讨会中，你更新了指令、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在不同对话中复用这些自定义设置，并根据团队需要调整。指令定义标准，技能描述可重复的任务，自定义智能体定义专业角色，MCP 服务器连接外部工具。审查实际更改和工具结果，而不只是智能体摘要。

根据任务选择**模式和模型**：使用 **Plan** 先制定方案，使用 **Interactive** 处理需要持续参与的专注更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可选用速度更快的模型，复杂任务可选用能力更强的模型。

基础仍然是清晰的上下文。明确说明要构建**什么**、**为什么**以及**如何**构建，会显著影响结果。

## 进一步探索

核心工作流已完成。还可以继续了解 Copilot CLI 的功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一个视角。
- `/fleet`：协调多个独立子任务并行执行。
- `/worktree`：隔离其他任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

熟练使用工具的最佳方式是持续实践。可以在生产代码、个人项目或一直想构建的小应用中使用 Copilot。与团队分享经验，向他人学习，并继续探索文档。

如需从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。也可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
