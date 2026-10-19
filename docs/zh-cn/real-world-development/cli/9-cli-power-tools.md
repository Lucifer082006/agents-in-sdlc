---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他 CLI 工具一样，GitHub Copilot CLI 包含许多用于交互的斜杠命令，可提供高级功能、幕后信息和其他配置选项。你已使用 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。本课将介绍其他实用命令。

本课将学习使用 `/context` 和 `/compact` 管理对话上下文、使用 `/model` 查看模型、使用 `/share` 导出或共享会话，以及了解并行工作、工作树和委派至云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。接下来了解上下文管理、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，也可以通过斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用情况，以及上下文在系统指令、工具、消息和剩余空间之间的分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口快满时自动压缩。需要自行选择压缩时机时，使用 `/compact`；切换到无关任务并希望开启新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有不同偏好。Copilot CLI 可以列出并选择可用模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于提升 AI 工具的使用效果。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如需共享会话，选择适合的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 在发送给他人或公开发布前审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选的。不要公开团队不打算共享的存储库或对话内容。

## 可选：扩展或委派工作

核心研讨会已完成。Copilot CLI 还提供了适用于较大型任务的命令：

- `/fleet` 将独立子任务分配给子智能体并行处理。
- `/worktree` 为独立任务创建隔离的 Git 工作树。
- `/delegate` 将任务发送给 Copilot 云智能体，让它异步工作并可能创建拉取请求。

这些命令可能会创建额外的工作树或远程任务，因此属于可选功能。尝试前先开始一个范围明确的新任务，并通过常规工作流审查结果。如需深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

斜杠命令可用于配置 Copilot CLI、共享会话并了解幕后运行情况。本课使用 `/context` 和 `/compact` 探索上下文管理，使用 `/model` 查看模型，学习使用 `/share` 导出或共享会话，并了解用于并行工作、工作树和云端委派的可选命令。

Copilot CLI 还有其他斜杠命令等待探索。接下来[回顾所学内容][next-lesson]和后续学习资源。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
