# 👥 CrewAI — 角色扮演多 Agent 框架

> [GitHub](https://github.com/crewAIInc/crewAI) | [文档](https://docs.crewai.com/) | Stars: 28k+ | 语言: Python

## 一句话概述

CrewAI 用**角色（Agent）+ 任务（Task）+ 团队（Crew）**三层抽象，像组建一支团队一样编排 AI。是目前**上手最快**的多 Agent 框架。

---

## 🏗️ 核心架构

```
┌─────────────────── Crew ────────────────────┐
│                                              │
│  Agent(研究员) → Task(调研)                   │
│       ↓ 输出传递                              │
│  Agent(作家)   → Task(写作)                   │
│       ↓                                      │
│  Agent(编辑)   → Task(审校)                   │
│                                              │
│  执行模式: sequential / hierarchical          │
└──────────────────────────────────────────────┘
```

---

## 🎯 核心概念

| 概念 | 说明 | 类比 |
|------|------|------|
| **Agent** | 有角色、目标、背景故事的 AI 角色 | 团队成员 |
| **Task** | 具体要完成的任务，绑定到 Agent | 工作任务单 |
| **Crew** | Agent + Task 的组合，定义执行顺序 | 整个团队 |
| **Tool** | Agent 可使用的外部工具 | 办公工具 |
| **Process** | 执行模式（顺序/层级/共识） | 管理方式 |

---

## 🚀 快速入门

```bash
pip install crewai crewai-tools
```

```python
from crewai import Agent, Task, Crew, Process

# 定义 Agent
researcher = Agent(
    role="AI 行业研究员",
    goal="深度调研 AI Agent 框架的最新发展趋势",
    backstory="你是一名资深 AI 行业分析师，擅长技术趋势分析。",
    verbose=True
)

writer = Agent(
    role="技术作家",
    goal="将研究结果写成通俗易懂的技术文章",
    backstory="你是一名科技媒体编辑，擅长将复杂技术概念讲清楚。"
)

# 定义任务
research_task = Task(
    description="调研 2026 年 AI Agent 框架的发展趋势，重点关注 Scion、AutoGen、CrewAI。",
    expected_output="一份 500 字的趋势分析报告",
    agent=researcher
)

writing_task = Task(
    description="基于调研报告，写一篇面向开发者的技术博客。",
    expected_output="一篇 800 字的技术博客文章",
    agent=writer
)

# 组建团队
crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, writing_task],
    process=Process.sequential  # 顺序执行
)

result = crew.kickoff()
print(result)
```

---

## ✅ 优势

1. **最简单的 API** — 学习曲线最平缓，30 分钟上手
2. **角色扮演直觉** — "像组建团队一样" 编排 AI
3. **丰富的工具生态** — crewai-tools 提供搜索、爬虫、文件等工具
4. **内置记忆** — 支持短期/长期/实体记忆
5. **A2A 协议支持** — 正在接入 Google A2A 协议

## ❌ 劣势

1. **深度定制受限** — 固定的执行模式，灵活性不如 LangGraph
2. **异步支持弱** — 并行执行能力有限
3. **错误处理粗糙** — Agent 失败时的回退机制不够完善
4. **Token 效率** — backstory 等描述消耗额外 token

## 🎯 适用场景

| 适合 | 不适合 |
|------|--------|
| 内容创作流水线 | 需要精确控制每一步的场景 |
| 数据调研 + 报告生成 | 大规模 Agent 集群 |
| 快速原型验证 | 需要复杂条件分支的工作流 |
| 团队刚接触 Agent 开发 | 需要分布式部署 |
