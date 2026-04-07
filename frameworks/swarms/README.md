# 🐝 Swarms — 群体智能 Agent 框架

> [GitHub](https://github.com/kyegomez/swarms) | [文档](https://docs.swarms.world/) | Stars: 4k+ | 语言: Python

## 一句话概述

Swarms 支持**数百个 Agent 并发运行**，提供顺序、并行、层级、混合等多种编排模式，适合大规模 Agent 集群场景。

---

## 🏗️ 核心架构

```
┌────────────────────────────────────┐
│          Orchestrator              │
│  ┌─────────────────────────────┐  │
│  │    SequentialWorkflow       │  │
│  │    ConcurrentWorkflow       │  │
│  │    AgentRearrange           │  │
│  │    MixtureOfAgents          │  │
│  │    SpreadSheetSwarm         │  │
│  └─────────────────────────────┘  │
│       ↕           ↕          ↕    │
│  ┌──────┐   ┌──────┐   ┌──────┐ │
│  │Agent │   │Agent │...│Agent │ │
│  │  1   │   │  2   │   │ 100  │ │
│  └──────┘   └──────┘   └──────┘ │
└────────────────────────────────────┘
```

---

## 🎯 编排模式

| 模式 | 说明 | 适用场景 |
|------|------|---------|
| **SequentialWorkflow** | 依次执行 | 流水线任务 |
| **ConcurrentWorkflow** | 并行执行 | 独立任务批处理 |
| **AgentRearrange** | 自定义拓扑 | 复杂依赖关系 |
| **MixtureOfAgents** | 多 Agent 投票 | 提高输出质量 |
| **SpreadSheetSwarm** | 表格数据并行 | 批量数据处理 |

---

## 🚀 快速入门

```bash
pip install swarms
```

```python
from swarms import Agent, SequentialWorkflow

# 创建 Agent
analyst = Agent(
    agent_name="数据分析师",
    system_prompt="你是一名数据分析师，擅长从数据中提取 insight。",
    model_name="gpt-4o"
)
writer = Agent(
    agent_name="报告撰写",
    system_prompt="你是一名技术作家，擅长撰写数据分析报告。",
    model_name="gpt-4o"
)

# 顺序工作流
workflow = SequentialWorkflow(
    agents=[analyst, writer],
    name="数据报告流水线"
)

result = workflow.run("分析这份 Q1 销售数据并生成报告")
```

```python
# 并行工作流 — 100 个 Agent 同时处理
from swarms import Agent, ConcurrentWorkflow

agents = [
    Agent(agent_name=f"worker-{i}", system_prompt="分析文档并提取关键信息", model_name="gpt-4o-mini")
    for i in range(100)
]

workflow = ConcurrentWorkflow(agents=agents)
results = workflow.run("提取关键信息")
```

---

## ✅ 优势

1. **规模大** — 支持数百 Agent 并发
2. **模式丰富** — 多种编排模式覆盖各种拓扑
3. **简单直接** — API 设计简洁
4. **SpreadSheet 模式** — 独特的表格批处理能力

## ❌ 劣势

1. **社区较小** — 4k Stars，生态不如 AutoGen/CrewAI
2. **文档质量** — 文档不够完善
3. **稳定性** — 大规模并发时可能有 edge case
4. **Token 成本** — 100 个 Agent 并发的费用可观

## 🎯 适用场景

| 适合 | 不适合 |
|------|--------|
| 大规模文档批处理 | 2-3 个 Agent 的简单场景 |
| 并行数据分析 | 预算敏感的项目 |
| 投票/共识决策 | 需要精确流程控制的场景 |
| 批量内容生成 | 需要 Agent 深度交互的场景 |
