# 🌱 Scion — Google Cloud 多 Agent 容器编排测试平台

> **首发中文深度解读** | 开源时间：2026 年 4 月 | [GitHub](https://github.com/GoogleCloudPlatform/scion) | [文档](https://googlecloudplatform.github.io/scion/)

## 一句话概述

Scion 让你**在独立容器中并行运行多个 AI 编码 Agent**（Claude Code、Gemini CLI、Codex 等），每个 Agent 有自己的工作空间和凭证，互不干扰。

> sci·on /ˈsīən/ — 嫩枝，用于嫁接或扦插。

---

## 🏗️ 核心架构

```
┌──────────────────────────────────────────┐
│                  Hub（可选）               │
│        中央控制平面 / Web Dashboard        │
│     身份认证 · 状态持久化 · 调度编排        │
└────────────┬─────────────┬───────────────┘
             │             │
    ┌────────▼────┐  ┌─────▼────────┐
    │Runtime Broker│  │Runtime Broker │
    │  (笔记本)    │  │   (K8s 集群)  │
    └──┬───┬───┬──┘  └──┬───┬───┬───┘
       │   │   │        │   │   │
      ┌▼┐ ┌▼┐ ┌▼┐     ┌▼┐ ┌▼┐ ┌▼┐
      │A│ │A│ │A│     │A│ │A│ │A│  ← Agent 容器
      │1│ │2│ │3│     │4│ │5│ │6│
      └─┘ └─┘ └─┘     └─┘ └─┘ └─┘
       ↕   ↕   ↕       ↕   ↕   ↕
    独立 git worktree + 独立凭证 + tmux 会话
```

### 六大核心概念

| 概念 | 说明 | 类比 |
|------|------|------|
| **Agent** | 容器化的 LLM 进程（Claude Code / Gemini CLI 等） | 一个"员工" |
| **Grove** | 项目工作空间，对应 `.scion/` 目录 | 一个"办公室" |
| **Template** | Agent 蓝图：系统 prompt + 技能集 + 配置 | "岗位说明书" |
| **Runtime** | 容器运行时：Docker / Podman / Apple Container / K8s | "工位类型" |
| **Hub** | 中央控制平面（可选），多机器 + 多用户协调 | "总部大楼" |
| **Runtime Broker** | 算力节点，向 Hub 注册提供执行能力 | "分公司" |

---

## 🎯 核心特性

### 1. 真正的隔离

每个 Agent 运行在**独立容器**中：
- 独立文件系统
- 独立 git worktree（不会产生 merge 冲突）
- 独立凭证和环境变量
- 独立 tmux 会话

### 2. 多 Agent 并行

```bash
# 同时启动 3 个 Agent 处理不同任务
scion start frontend "实现登录页面 UI"
scion start backend "设计 REST API"
scion start tests "写集成测试"

# 查看所有 Agent 状态
scion list
```

### 3. Attach / Detach

Agent 在后台运行，随时可以 attach 进行交互：

```bash
# 接入 Agent 的 tmux 会话
scion attach frontend

# 发送消息（不用 attach）
scion message backend "API 需要支持分页"

# 查看日志
scion logs tests
```

### 4. Harness 无关

支持多种底层 Agent 引擎，统一管理接口：

| Harness | 说明 |
|---------|------|
| Claude Code | Anthropic 的编码 Agent |
| Gemini CLI | Google 的 Gemini Agent |
| Codex | OpenAI 的编码 Agent |
| OpenCode | 开源编码 Agent |

### 5. 多运行时

| 运行时 | 平台 | 特点 |
|--------|------|------|
| Docker | Linux/macOS | 标准方案 |
| Podman | Linux/macOS | 无 daemon，rootless |
| Apple Container | macOS | 原生虚拟化框架，性能好 |
| Kubernetes | 任意 | 生产级扩展 |

---

## 🚀 快速入门

### 安装

```bash
# 需要 Go 环境
go install github.com/GoogleCloudPlatform/scion/cmd/scion@latest

# 初始化机器（构建容器镜像）
scion init --machine

# 在项目中初始化 Grove
cd my-project
scion init
```

> ⚠️ 目前还不提供预构建二进制文件，需要从源码安装。

### Hello World

```bash
# 启动一个 Agent 并 attach
scion start helper "帮我写一个 Python hello world 程序" --attach

# 或者后台启动
scion start helper "帮我写一个 Python hello world 程序"

# 查看状态
scion list

# 稍后 attach
scion attach helper
```

### 团队协作示例

```bash
# 定义 Agent 模板
cat > .scion/templates/security-auditor.yaml << 'EOF'
name: security-auditor
harness: claude
system_prompt: |
  你是一名安全审计专家。检查代码中的安全漏洞，
  包括 SQL 注入、XSS、CSRF、认证绕过等问题。
  输出结构化的审计报告。
EOF

# 使用模板启动
scion start audit --template security-auditor "审计 src/ 目录的所有代码"

# 同时启动开发 Agent
scion start dev "修复审计报告中发现的问题"

# Agent 间通信
scion message dev "安全审计发现了 3 个高危漏洞，详见 audit-report.md"
```

---

## 🔧 Agent 状态模型

Scion 用三层模型跟踪 Agent 状态：

| 层级 | 说明 | 可能值 |
|------|------|--------|
| **Phase** | 容器生命周期 | created → provisioning → cloning → starting → **running** → stopping → stopped / error |
| **Activity** | Agent 认知状态 | idle, thinking, executing, waiting_for_input, blocked, completed, limits_exceeded, offline |
| **Detail** | 自由文本 | "正在执行 npm test", "等待用户确认" 等 |

---

## 📊 与其他框架对比

| 维度 | Scion | AutoGen | CrewAI |
|------|-------|---------|--------|
| **隔离级别** | 容器级 | 进程级 | 线程级 |
| **并行模式** | 真并行（独立容器） | 协程/多进程 | 协程 |
| **协作方式** | CLI 工具 + 消息 | 对话消息 | 任务链 |
| **底层 Agent** | 任意 harness | 自带 Agent | 自带 Agent |
| **适合规模** | 2-100+ Agent | 2-10 Agent | 2-10 Agent |
| **运维复杂度** | 高（需要容器环境） | 中 | 低 |

---

## ✅ 优势

1. **真正的隔离** — 容器级隔离，不怕 Agent 互相干扰
2. **Harness 无关** — 不绑定特定 LLM 提供商
3. **可扩展** — 从笔记本到 K8s 集群无缝扩展
4. **Google 背书** — Google Cloud Platform 开源，有持续投入
5. **Human-in-the-Loop** — tmux attach 随时介入

## ❌ 劣势

1. **实验阶段** — 官方明确标注 "early and experimental"
2. **安装门槛高** — 需要 Go 环境 + 手动构建容器镜像
3. **学习曲线陡** — Grove / Hub / Runtime Broker 概念较多
4. **文档不完善** — 刚开源，文档还在完善中
5. **不适合简单场景** — 如果只需要 2 个 Agent 聊天，杀鸡用牛刀

## 🎯 适用场景

| 适合 | 不适合 |
|------|--------|
| 多 Agent 并行编码 | 简单的问答 Bot |
| 需要严格隔离的安全审计 | 快速原型验证 |
| 大型项目的分模块开发 | 非技术团队使用 |
| K8s 原生的 AI 工作负载 | 不想管容器的团队 |
| 同时使用多种 LLM 的场景 | 预算有限的小团队 |

---

## 🔮 展望

Scion 目前处于实验阶段，但它代表了一种重要趋势：**Agent 从"聊天"走向"工作"**。当 Agent 需要操作文件系统、运行命令、修改代码时，容器级隔离不是可选的——是必须的。

随着 Google Cloud 的持续投入，Scion 有望成为企业级多 Agent 编排的标准方案之一。

---

## 📚 参考链接

- [GitHub Repo](https://github.com/GoogleCloudPlatform/scion)
- [官方文档](https://googlecloudplatform.github.io/scion/)
- [概念详解](https://googlecloudplatform.github.io/scion/concepts/)
- [安装指南](https://googlecloudplatform.github.io/scion/getting-started/install/)
- [InfoQ 报道](https://www.infoq.com/news/2026/04/google-agent-testbed-scion/)
- [Hacker News 讨论](https://news.ycombinator.com/item?id=47675213)
- [Relics of Athenaeum — Agent 协作游戏 Demo](https://github.com/ptone/scion-athenaeum)
