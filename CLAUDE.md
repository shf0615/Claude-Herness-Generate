# Harness Generate v2 — 交互式工作空间管理系统

> 通过多轮问答，与用户共同**创建、修改、审计、优化** Claude Code harness 工作空间。
> 每一步都需要用户确认，绝不自动执行。

## 核心能力

| 能力 | 命令 | 代理 | 说明 |
|------|------|------|------|
| 创建新工作空间 | `/create` | creator | 7 阶段问答，从零设计并生成 .claude/ |
| 修改已有工作空间 | `/modify [path]` | modifier | 分析→建议→预览→确认→执行 |
| 审计工作空间 | `/audit [path]` | auditor | 健康检查、一致性、安全性诊断 |
| 智能推荐 | `/recommend [path]` | recommender | 分析代码→推荐最佳配置 |

## 路由规则

| 用户意图 | 路由到 | 触发词 |
|----------|--------|--------|
| 创建新工作空间 | **creator** | 创建、新建、生成、初始化 |
| 修改已有配置 | **modifier** | 修改、调整、更新、添加、删除 |
| 检查配置健康度 | **auditor** | 检查、审计、诊断、健康 |
| 代码分析推荐 | **recommender** | 推荐、建议、智能分析 |
| 意图不明确 | **dispatcher** | 其他 |

## 代理阵容

| 代理 | 职责 | 模型 | 权限 |
|------|------|------|------|
| dispatcher | 意图分析与路由 | sonnet | 只读 |
| creator | 创建新工作空间 | sonnet | 读写 |
| modifier | 修改已有工作空间 | sonnet | 读写 |
| auditor | 审计诊断 | sonnet | 只读 + Bash |
| recommender | 智能推荐 | sonnet | 只读 + Bash |

## 核心设计原则

1. **问答优先** — 每个阶段 ⛔ STOP，等用户确认
2. **不跨阶段** — 一个回复只做一个阶段
3. **不替用户决定** — 始终提供选项
4. **契约文件模式** — Agent 间通过 `docs/contracts/` 传递上下文
5. **开放式输入** — 不限定项目类型和技术栈

## 契约文件模式

生成的工作空间中，agents 通过契约文件协作：

```
docs/contracts/
├── {source}-to-{target}.md    ← 结构化的上下文传递文件
└── ...
```

每个契约文件定义：源 agent、目标 agent、数据格式、更新时机。
Agent prompt 中包含读写契约的指令，确保信息在代理间正确流转。

## 快速开始

```bash
cd ~/workspace-ai/claude/Claude-Herness-Generate2
claude

# 创建新工作空间
/create

# 修改已有工作空间
/modify /path/to/project

# 审计工作空间
/audit /path/to/project

# 智能推荐
/recommend /path/to/project
```
