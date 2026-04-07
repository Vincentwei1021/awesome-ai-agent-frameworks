# 🧭 AI Agent 框架选型决策指南

> 回答几个关键问题，找到最适合你的框架。

## 决策流程图

```mermaid
flowchart TD
    Start([🚀 开始选型]) --> Q1{需要写代码吗？}

    Q1 -->|完全不想写代码| NoCoder{需要私有化部署吗？}
    NoCoder -->|不需要，SaaS 就行| Coze[✅ <b>Coze 扣子</b><br/>零代码 + 丰富插件<br/>直接发布到飞书/微信]
    NoCoder -->|需要部署到自己服务器| Dify[✅ <b>Dify</b><br/>开源可视化平台<br/>Docker 一键部署]

    Q1 -->|可以写代码| Q2{核心需求是什么？}

    Q2 -->|多 Agent 对话协作| Q3{需要分布式部署吗？}
    Q3 -->|是，跨机器/Kubernetes| AutoGen[✅ <b>AutoGen v0.4</b><br/>事件驱动 + 分布式<br/>微软出品]
    Q3 -->|否，单机就够| CrewAI[✅ <b>CrewAI</b><br/>角色 + 任务 + 团队<br/>最快上手]

    Q2 -->|复杂工作流编排| LangGraph[✅ <b>LangGraph</b><br/>状态图 + 条件分支<br/>人类介入 + 持久化]

    Q2 -->|模拟软件开发流程| Q4{需要什么级别的隔离？}
    Q4 -->|代码级隔离就行| MetaGPT[✅ <b>MetaGPT</b><br/>SOP 驱动<br/>自动 PRD → 代码]
    Q4 -->|容器级隔离，并行编码| Scion[✅ <b>Scion</b> 🆕<br/>Google Cloud 出品<br/>容器编排 + git worktree]

    Q2 -->|大规模 Agent 集群| Swarms[✅ <b>Swarms</b><br/>群体智能<br/>百级 Agent 并发]

    Q2 -->|快速搭建 AI 应用| Q5{技术栈偏好？}
    Q5 -->|Python 为主| CrewAI2[✅ <b>CrewAI</b>]
    Q5 -->|可视化优先| Dify2[✅ <b>Dify</b>]

    style Coze fill:#10B981,color:#fff,stroke:#059669
    style Dify fill:#10B981,color:#fff,stroke:#059669
    style AutoGen fill:#3B82F6,color:#fff,stroke:#2563EB
    style CrewAI fill:#3B82F6,color:#fff,stroke:#2563EB
    style LangGraph fill:#8B5CF6,color:#fff,stroke:#7C3AED
    style MetaGPT fill:#F59E0B,color:#fff,stroke:#D97706
    style Scion fill:#EF4444,color:#fff,stroke:#DC2626
    style Swarms fill:#EC4899,color:#fff,stroke:#DB2777
    style CrewAI2 fill:#3B82F6,color:#fff,stroke:#2563EB
    style Dify2 fill:#10B981,color:#fff,stroke:#059669
```

---

## 关键决策因素

### 1. 团队技术水平

| 水平 | 推荐路径 |
|------|---------|
| **非技术人员** | Coze（纯界面）→ Dify（低代码） |
| **初级开发者** | CrewAI（最简 API）→ AutoGen（渐进复杂） |
| **高级开发者** | LangGraph（精确控制）→ Scion（容器编排） |
| **AI 研究者** | AutoGen（可定制对话）→ MetaGPT（SOP 实验） |

### 2. 部署环境

| 环境 | 推荐 |
|------|------|
| **纯 SaaS** | Coze |
| **Docker 单机** | Dify / CrewAI / AutoGen |
| **Kubernetes 集群** | Scion / AutoGen v0.4 |
| **笔记本/本地开发** | CrewAI / LangGraph |

### 3. Agent 数量

| 规模 | 推荐 |
|------|------|
| **1-3 个 Agent** | CrewAI / LangGraph |
| **3-10 个 Agent** | AutoGen / MetaGPT / Scion |
| **10-100+ 个 Agent** | Swarms / Scion（K8s 模式） |

### 4. 是否需要人类介入

| 需求 | 推荐 |
|------|------|
| **完全自动** | CrewAI / MetaGPT / Swarms |
| **关键节点审批** | LangGraph（内置 human-in-the-loop） |
| **随时 attach 交互** | Scion（tmux attach/detach） |

---

## 组合方案

有时候不是非此即彼，框架可以组合使用：

| 组合 | 场景 |
|------|------|
| **Dify + LangGraph** | 可视化管理 + 复杂内部逻辑 |
| **Scion + CrewAI** | 容器隔离 + 内部用 CrewAI 编排 |
| **AutoGen + LangGraph** | 对话协作 + 工作流控制 |
| **Coze + Dify** | 前端快速搭建 + 后端自研能力 |

---

## 总结一句话

| 框架 | 一句话 |
|------|--------|
| **Scion** | "给每个 Agent 一个容器，让它们自己学会协作" |
| **AutoGen** | "Agent 之间像人一样聊天解决问题" |
| **CrewAI** | "组一支 AI 团队，分工明确，开干" |
| **LangGraph** | "画一张图，精确控制每一步" |
| **MetaGPT** | "模拟一个软件公司，从 PRD 到代码全自动" |
| **Swarms** | "放出一群 Agent，蚁群智能" |
| **Dify** | "拖拖拽拽就能搭 AI 应用" |
| **Coze** | "最快 5 分钟上线一个 AI Bot" |
