# 🤝 贡献指南

感谢你有兴趣为本项目做贡献！

## 如何贡献

### 1. 修正错误

如果发现内容有误（数据过时、描述不准确、代码示例有 bug），欢迎直接提 PR。

### 2. 添加新框架

想添加新的 Agent 框架？请按以下模板：

```
frameworks/<框架名>/
└── README.md
```

README.md 需要包含：
- 一句话概述
- 核心架构图（ASCII art 或 Mermaid）
- 核心概念表格
- 快速入门代码示例
- 优势 / 劣势
- 适用场景

### 3. 更新对比数据

框架在快速迭代，如果发现对比数据过时，欢迎更新。请在 PR 中注明数据来源。

## PR 规范

1. Fork 本仓库
2. 创建分支：`git checkout -b feat/add-xxx-framework`
3. 提交：`git commit -m "feat: 添加 XXX 框架详解"`
4. 推送：`git push origin feat/add-xxx-framework`
5. 创建 Pull Request

## 内容规范

- **语言**：中文为主，代码注释可中英混用
- **客观**：不做商业推广，客观描述优缺点
- **实用**：代码示例需可运行，不要伪代码
- **更新**：注明数据截至日期

## 行为准则

- 尊重所有贡献者
- 欢迎建设性讨论
- 不做人身攻击

---

有任何问题？欢迎在 [Discussions](https://github.com/Vincentwei1021/awesome-ai-agent-frameworks/discussions) 中讨论。
