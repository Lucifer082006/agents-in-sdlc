---
title: "第 8 课 - 创建并合并功能 PR"
description: "审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支中。现在一起审查这些更改并创建拉取请求。此前你手动合并了星级评分 PR；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分为几个 PR。为简化研讨会，本次筛选与质量工作流在同一对话和分支中完成，并合并到一个 PR。

本课将介绍 Agent Merge 如何自动处理合并生命周期、如何检查完整功能更改和验证证据、如何创建筛选 PR，以及如何在审查后启用 Agent Merge 并确认 PR 已合并。

## 场景

整个筛选功能工作流中，你都使用 Copilot 规划、实现和验证功能。现在 Tailspin Toys 希望自动化剩余 PR 工作，同时仍由开发者控制合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的后续工作。启用后，Copilot 会处理阻碍 PR 合并的事项，包括修复持续集成 (CI) 检查失败、响应审查评论，并在需要时执行变基。之后，如果存储库允许，它会启用 GitHub 自动合并。

之前一直由你选择 **Merge pull request**。启用 Agent Merge 前，仍需先审查工作并确认准备就绪。

## 使用 Agent Merge 管理 PR

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程；所有检查通过后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限要求。如果操作受阻，请查看报告的原因，并在存储库允许时手动完成经审查的合并。

## 总结与后续步骤

你已自动化开发流程的多个部分，包括代码生成、测试和验证，以及拉取请求流程。本课介绍了 Agent Merge 的自动合并生命周期、审查完整功能更改和验证证据、创建筛选 PR，并在审查后启用 Agent Merge。

接下来探索用于管理上下文、模型、共享和可选云端委派的实用 [Copilot CLI 斜杠命令][next-lesson]。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
