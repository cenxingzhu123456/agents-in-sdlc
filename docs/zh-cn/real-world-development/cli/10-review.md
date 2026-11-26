---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的完整工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目，并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令指导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol（MCP）服务器，并在真实浏览器中试用了筛选功能。
- 创建并选择了质量保证（QA）自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，分别基于更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示尚未评分的状态。
2. **筛选和质量工作流**：实现筛选、更新并应用功能指令、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为了简化研讨会流程，这些工作合并为一个 PR。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 浏览器探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在创建 PR 前汇总这些结果，并审查需求和测试覆盖率。

新增测试应解决实际存在的缺口；如果 QA 检查后认为无需添加测试，也可能是合理结果。授权合并前，请审查代码和证据；更改后请重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令规定标准，技能描述可重复执行的任务，自定义智能体定义专家角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择**模式和模型**。使用 **Plan** 在实现前考虑方案，使用 **Interactive** 参与专注的小型更改，并对范围明确的任务使用 **Autopilot**。常规编辑可选速度较快的模型，复杂工作则可选能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 深入探索

你已学习核心工作流。还可以了解一些 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种看法。
- 使用 `/fleet` 并行协调互不依赖的子任务。
- 使用 `/worktree` 隔离独立任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方法就是持续使用它！可将其用于生产代码、个人项目，或多年来一直想构建却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续阅读文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
