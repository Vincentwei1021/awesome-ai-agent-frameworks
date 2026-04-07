# 🤖 AutoGen — Microsoft 对话驱动多 Agent 框架

> [GitHub](https://github.com/microsoft/autogen) | [文档](https://microsoft.github.io/autogen/) | Stars: 42k+ | 语言: Python

## 一句话概述

AutoGen 让多个 AI Agent 通过**自然语言对话**协作完成任务，就像一群人在群聊中讨论问题。

---

## 🏗️ 核心架构

```
┌─────────────────────────────────┐
│         GroupChat / Team         │
│  ┌──────┐ ┌──────┐ ┌──────┐   │
│  │Agent │←→│Agent │←→│Agent │   │
│  │  A   │  │  B   │  │  C   │   │
│  └──────┘ └──────┘ └──────┘   │
│       ↕ 消息传递 (对话) ↕       │
└─────────────────────────────────┘
```

**v0.4 重大重构**：从同步对话模式升级为**事件驱动架构**，支持分布式部署。

---

## 🎯 核心概念

| 概念 | 说明 |
|------|------|
| **AssistantAgent** | 基础 Agent，可调用 LLM 和工具 |
| **UserProxyAgent** | 代表人类的 Agent，可执行代码 |
| **GroupChat** | 多 Agent 对话组 |
| **Team** | Agent 团队（RoundRobin / Selector / Swarm） |
| **Termination** | 终止条件（文本匹配、最大回合等） |

---

## 🚀 快速入门

```bash
pip install autogen-agentchat autogen-ext[openai]
```

```python
import asyncio
from autogen_agentchat.agents import AssistantAgent
from autogen_agentchat.teams import RoundRobinGroupChat
from autogen_agentchat.conditions import TextMentionTermination
from autogen_ext.models.openai import OpenAIChatCompletionClient

async def main():
    model = OpenAIChatCompletionClient(model="gpt-4o")

    # 创建 Agent
    coder = AssistantAgent(
        "程序员",
        model_client=model,
        system_message="你是一个 Python 程序员，写简洁优雅的代码。"
    )
    reviewer = AssistantAgent(
        "审查员",
        model_client=model,
        system_message="你是代码审查员，检查代码质量和 bug。说 APPROVE 表示通过。"
    )

    # 组建团队
    termination = TextMentionTermination("APPROVE")
    team = RoundRobinGroupChat(
        [coder, reviewer],
        termination_condition=termination
    )

    # 开始任务
    result = await team.run(task="写一个快速排序算法")
    print(result)

asyncio.run(main())
```

---

## ✅ 优势

1. **微软背书** — 持续投入，社区活跃（42k+ Stars）
2. **灵活的对话模式** — Agent 自主决定何时发言、说什么
3. **代码执行** — 内置安全的代码执行环境
4. **分布式支持** — v0.4 支持跨机器部署 Agent
5. **丰富的团队模式** — RoundRobin、Selector、Swarm、MagenticOne

## ❌ 劣势

1. **输出不可控** — 对话模式下结果可能发散
2. **v0.2 → v0.4 迁移** — API 大改，老代码需重写
3. **调试困难** — 多 Agent 对话的 debug 较复杂
4. **Token 消耗高** — 多轮对话消耗大量 token

## 🎯 适用场景

| 适合 | 不适合 |
|------|--------|
| 多 Agent 研讨式任务 | 需要精确控制流程的场景 |
| 代码生成 + 自动执行 | Token 预算有限 |
| 需要人类介入的对话 | 非常简单的单步任务 |
| 分布式 Agent 协作 | 需要确定性输出的场景 |
