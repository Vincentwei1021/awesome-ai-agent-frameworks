# 🎨 Dify — 开源可视化 AI 应用开发平台

> [GitHub](https://github.com/langgenius/dify) | [官网](https://dify.ai/) | Stars: 62k+ | 语言: Python/TypeScript

## 一句话概述

Dify 提供**拖拽式画布**编排 Agent 工作流，内置 RAG 知识库、工具调用、对话管理，适合不想写太多代码的团队快速构建 AI 应用。

---

## 🏗️ 核心架构

```
┌──────────── Dify Platform ─────────────┐
│                                         │
│  ┌─────────────────────────────────┐   │
│  │     可视化 Workflow 画布          │   │
│  │  [输入] → [LLM] → [判断] → [输出] │   │
│  │            ↕                     │   │
│  │       [知识库检索]                │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ┌──────────┐ ┌──────────┐ ┌────────┐ │
│  │ 知识库管理│ │ 工具集成  │ │ 日志监控│ │
│  └──────────┘ └──────────┘ └────────┘ │
│                                         │
│  部署: Docker / Cloud                   │
└─────────────────────────────────────────┘
         │
    ┌────▼─────┐
    │ API / Web │ ← 一键发布
    └──────────┘
```

---

## 🎯 核心能力

| 能力 | 说明 |
|------|------|
| **Workflow** | 可视化拖拽编排 Agent 流程 |
| **RAG Pipeline** | 内置文档上传、分块、向量化、检索 |
| **Agent** | 支持 ReAct / Function Call 模式 |
| **Tools** | 内置工具 + 自定义 API 工具 |
| **多模型** | OpenAI / Claude / 本地模型（Ollama） |
| **发布** | 一键部署为 API 或嵌入式 Web 应用 |

---

## 🚀 快速部署

```bash
# Docker 一键部署
git clone https://github.com/langgenius/dify.git
cd dify/docker
cp .env.example .env
docker compose up -d
# 访问 http://localhost/install 完成初始化
```

### 创建 Agent Workflow（界面操作）

1. 创建应用 → 选择 "Workflow"
2. 拖拽节点：开始 → LLM → 条件判断 → 工具调用 → 结束
3. 配置每个节点的 prompt 和参数
4. 测试运行
5. 发布为 API

### API 调用

```python
import requests

response = requests.post(
    "http://your-dify-server/v1/workflows/run",
    headers={"Authorization": "Bearer YOUR_API_KEY"},
    json={
        "inputs": {"query": "帮我分析这份数据"},
        "response_mode": "streaming",
        "user": "user-123"
    }
)
```

---

## ✅ 优势

1. **62k+ Stars** — 目前最热门的开源 AI 应用平台
2. **零代码** — 拖拽式画布，非技术人员也能用
3. **RAG 内置** — 知识库管理开箱即用
4. **多模型** — 一键切换 OpenAI / Claude / 本地模型
5. **企业级** — 权限管理、日志审计、API 限流
6. **活跃社区** — 中文社区活跃，文档完善

## ❌ 劣势

1. **灵活性有限** — 可视化拖拽的表达能力不如代码
2. **复杂逻辑** — 图节点方式实现复杂业务逻辑比较痛苦
3. **性能** — 自托管需要不少资源（推荐 8GB+ 内存）
4. **厂商依赖** — 深度使用后迁移成本高

## 🎯 适用场景

| 适合 | 不适合 |
|------|--------|
| 企业内部 AI 知识库 | 需要复杂多 Agent 交互 |
| 客服机器人 | 高度定制化的 Agent 行为 |
| 内容审核工作流 | 预算极低（自托管需要资源） |
| 快速搭建 RAG 应用 | 需要精确控制 Agent 间通信 |
