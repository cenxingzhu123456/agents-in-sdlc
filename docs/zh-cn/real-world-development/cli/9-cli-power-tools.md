---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 包含许多斜杠命令。这些命令提供高级功能、幕后信息和额外配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来看看其他一些实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解更多功能，例如管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令自行检查或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 用量，以及上下文在系统指令、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口接近上限时自动压缩上下文。如果想自行选择压缩时机，请使用 `/compact`。切换到无关任务并希望开始新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并共享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，导出本地 Markdown 文件。
3. 发送给他人或发布之前，请审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心研讨会已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以将互不依赖的子任务分配给子智能体并行执行。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务交给 Copilot 云智能体异步处理，它可能会创建拉取请求。

这些命令是可选的，因为它们可能会创建额外工作树或触发远程工作。尝试前，请开始一个范围明确的新任务，并通过常规工作流审查结果。如果想深入了解异步智能体工作，可以继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 的斜杠命令，可以配置工具、共享会话，并了解后台运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树以及委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！现在通过[回顾所学内容][next-lesson]并了解后续学习建议，结束本次学习。

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
