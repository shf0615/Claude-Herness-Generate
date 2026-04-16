# Harness Generate v2

交互式 Claude Code Harness 工作空间管理系统。

通过多轮问答，与用户共同**创建、修改、审计、优化** Claude Code harness 工作空间。每一步都需要用户确认，绝不自动执行。

---

## 功能概览

| 能力 | 命令 | 说明 |
|------|------|------|
| **创建新工作空间** | `/create` | 7 阶段问答，从零设计并生成 `.claude/` 目录 |
| **修改已有工作空间** | `/modify [path]` | 分析 → 建议 → 预览 → 确认 → 执行 |
| **审计工作空间** | `/audit [path]` | 健康检查、一致性、安全性诊断 |
| **智能推荐** | `/recommend [path]` | 分析代码 → 推荐最佳配置 |

---

## 快速开始

```bash
cd ~/workspace-ai/claude/Claude-Herness-Generate
claude
```

### 创建新工作空间

```
/create
```

通过 7 阶段问答式流程，与用户共同设计：
1. 基础信息（项目名称、描述、技术栈、输出路径）
2. 代理阵容设计
3. 工作流编排
4. 契约文件设计
5. 命令 / 技能 / 自动化配置
6. 完整预览与确认
7. 生成全部文件

### 修改已有配置

```
/modify /path/to/project
```

### 审计工作空间

```
/audit /path/to/project
```

### 智能推荐

```
/recommend /path/to/project
```

---

## 项目结构

```
.
├── CLAUDE.md                 # 项目主控文档（路由规则 + 快速开始）
└── .claude/
    ├── settings.json         # 权限配置
    ├── rules/
    │   └── workspace-rules.md    # 工作空间设计规范
    ├── agents/               # 代理定义
    │   ├── creator.md
    │   ├── modifier.md
    │   ├── auditor.md
    │   ├── recommender.md
    │   └── dispatcher.md
    ├── commands/             # 斜杠命令入口
    │   ├── create.md
    │   ├── modify.md
    │   ├── audit.md
    │   └── recommend.md
    └── skills/
        └── harness-patterns/
            └── SKILL.md      # Harness 模式知识库
```

---

## 核心设计原则

1. **问答优先** — 每个阶段结束后必须 STOP，等待用户确认
2. **不跨阶段** — 一个回复只处理一个阶段
3. **不替用户决定** — 始终提供选项，让用户做最终选择
4. **契约文件模式** — Agent 间通过 `docs/contracts/` 传递上下文
5. **开放式输入** — 不限定项目类型和技术栈

---

## 代理阵容

| 代理 | 职责 | 模型 | 权限 |
|------|------|------|------|
| `dispatcher` | 意图分析与路由 | sonnet | 只读 |
| `creator` | 创建新工作空间 | sonnet | 读写 |
| `modifier` | 修改已有工作空间 | sonnet | 读写 |
| `auditor` | 审计诊断 | sonnet | 只读 + Bash |
| `recommender` | 智能推荐 | sonnet | 只读 + Bash |

---

## 契约文件模式

生成的工作空间中，agents 通过契约文件协作：

```
docs/contracts/
├── {source}-to-{target}.md    ← 结构化的上下文传递文件
└── ...
```

每个契约文件定义：源 agent、目标 agent、数据格式、更新时机。

---

## 路由规则

| 用户意图 | 路由到 | 触发词 |
|----------|--------|--------|
| 创建新工作空间 | **creator** | 创建、新建、生成、初始化 |
| 修改已有配置 | **modifier** | 修改、调整、更新、添加、删除 |
| 检查配置健康度 | **auditor** | 检查、审计、诊断、健康 |
| 代码分析推荐 | **recommender** | 推荐、建议、智能分析 |
| 意图不明确 | **dispatcher** | 其他 |

---

## 许可证

MIT
