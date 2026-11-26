---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三个旨在改善用户体验和添加功能的独立任务。云智能体虽可异步自主工作，但监控任务也很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求（PR）的智能体任务，了解已执行的操作，甚至可以引导仍在进行的会话。

本课将：

- 查看智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将实现高对比度模式的任务分配给智能体后，团队意识到也可以顺便添加浅色模式。由于已经在更新网站样式并添加切换功能，因此将此功能一并纳入很合理。现在要引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的工作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方法而变化。

4. 同时留意右侧显示的拉取请求（PR）面板。可在此查看 PR 和已更改文件，继续监控任务。

## 引导云智能体

现在你已查看正在进行的任务，可以请求 Copilot 在实现高对比度模式的同时添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到显示设置已完成并开始工作。看到类似下方的消息时，就表示已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中云智能体任务的屏幕截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！它会在完成你发送消息时正在执行的步骤后，将新请求加入工作流程。

和之前一样，这需要几分钟。可以利用这段时间暂停一下，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心控制台。使用此任务控制中心，可以跟踪所有正在进行和已完成的任务、审查正在执行的工作，甚至可以重定向仍在进行的任务，调整范围或补充指导。

你探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
