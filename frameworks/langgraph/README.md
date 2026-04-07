# 📊 LangGraph — 状态图工作流引擎

> [GitHub](https://github.com/langchain-ai/langgraph) | [文档](https://langchain-ai.github.io/langgraph/) | Stars: 12k+ | 语言: Python/JS

## 一句话概述

LangGraph 将 Agent 工作流建模为**有向图**，节点是处理步骤，边是条件转移。**精确控制每一步**，支持持久化状态和人类介入。

---

## 🏗️ 核心架构

```
         ┌─────────┐
         │  START   │
         └────┬─────┘
              │
         ┌────▼─────┐
    ┌────│ 研究节点  │────┐
    │    └──────────┘    │
    │ (信息足够)    (需要更多) │
    │                    │
┌───▼────┐        ┌─────▼───┐
│ 写作节点│        │ 搜索节点 │──→ 回到研究
└───┬────┘        └─────────┘
    │
┌───▼────┐
│ 审核节点│──→ (不通过) → 回到写作
└───┬────┘
    │ (通过)
┌───▼────┐
│  END   │
└────────┘
```

---

## 🎯 核心概念

| 概念 | 说明 |
|------|------|
| **StateGraph** | 状态图，定义整个工作流 |
| **Node** | 图中的节点，执行具体操作 |
| **Edge** | 节点之间的连接（普通/条件） |
| **State** | 在节点间传递的状态对象 |
| **Checkpointer** | 状态持久化，支持断点恢复 |
| **Human-in-the-loop** | 在关键节点暂停等待人类确认 |

---

## 🚀 快速入门

```bash
pip install langgraph langchain-openai
```

```python
from typing import TypedDict, Annotated
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages

# 定义状态
class State(TypedDict):
    messages: Annotated[list, add_messages]
    research_done: bool

# 定义节点
def research(state: State) -> State:
    """研究节点：收集信息"""
    # ... 调用 LLM 或工具
    return {"messages": [("assistant", "研究完成")], "research_done": True}

def write(state: State) -> State:
    """写作节点：生成内容"""
    return {"messages": [("assistant", "文章已生成")]}

def should_continue(state: State) -> str:
    """条件边：决定下一步"""
    if state["research_done"]:
        return "write"
    return "research"

# 构建图
graph = StateGraph(State)
graph.add_node("research", research)
graph.add_node("write", write)
graph.add_edge(START, "research")
graph.add_conditional_edges("research", should_continue)
graph.add_edge("write", END)

# 编译并运行
app = graph.compile()
result = app.invoke({"messages": [], "research_done": False})
```

---

## ✅ 优势

1. **精确控制** — 图结构清晰定义每一步的逻辑
2. **状态持久化** — Checkpointer 支持断点续跑
3. **Human-in-the-Loop** — 内置人类审批节点
4. **LangChain 生态** — 无缝对接 LangChain 的工具和模型
5. **可视化** — LangGraph Studio 可视化调试
6. **LangGraph Cloud** — 一键部署为 API 服务

## ❌ 劣势

1. **学习曲线陡** — 图编程范式需要适应
2. **代码量大** — 简单任务也需要定义图结构
3. **不够 "智能"** — 流程是预定义的，不如对话模式灵活
4. **依赖 LangChain** — 和 LangChain 生态深度耦合

## 🎯 适用场景

| 适合 | 不适合 |
|------|--------|
| 需要精确控制流程的工作流 | 简单的多 Agent 聊天 |
| 需要人类审批的关键节点 | 快速原型验证 |
| 长时间运行的任务（断点续跑） | Agent 需要自主决策路径的场景 |
| 复杂的条件分支逻辑 | 初学者 |
