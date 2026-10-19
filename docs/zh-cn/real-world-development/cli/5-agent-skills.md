---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有 quality-checks 技能、自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码并非开发工作的全部。我们已经手动验证代码，并使用指令文件确保遵循标准。接下来还要考虑测试、lint 和其他持续集成 (CI) 工作。**智能体技能**很适合这类任务，可以指导 Copilot 正确执行操作。

本课将探索现有 `quality-checks` 技能、自定义结果格式并重新加载和运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改善报告，使结果更易于阅读。

## 指令、脚本和资源

智能体技能会打包可复用的任务指令、可执行脚本和支持资源，供智能体按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含 `SKILL.md`。Markdown 文件的 frontmatter 指定技能名称和说明，并提供功能概述及调用时机指导。文件夹还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目现有命令。

技能可以存放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能目录 `~/.copilot/skills` 中。

## 探索技能

Tailspin Toys 团队创建了名为 `quality-checks` 的技能，用于运行单元测试、lint 和类型检查。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用技能。
3. 阅读指令，了解它如何引导 Copilot 执行测试和 lint。
4. 注意技能尚未包含 **Results output formatting** 部分。

## 修改前先运行技能

可以通过 Copilot CLI 命令或自然语言调用技能。现在让 Copilot 运行三项检查：

1. 返回处于 Interactive 模式的筛选对话。
2. 发送提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

改进报告，使其说明运行了哪些检查、是否成功以及工具实际报告了什么。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次发送与之前相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看并比较两次报告。

## 总结与后续步骤

你已探索用于运行单元测试、lint 和类型检查的 `quality-checks` 技能，自定义其结果格式，并重新加载和运行技能。这项更改会随筛选功能一起进入功能 PR。接下来通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
