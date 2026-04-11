# 通用格式参考

> 此文件供 harness-expert 代理在生成文件时参考格式。不含具体内容——所有内容由问答决定。

## Agent 文件格式

```markdown
---
name: agent-name
description: 何时使用此代理以及它做什么
tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]
model: sonnet
---

# 代理标题

## 角色
一段话描述代理的职责。

## 工作流
分步操作说明。

## 质量检查
- [ ] 检查项 1
- [ ] 检查项 2
```

**模型选择：**
- `haiku` — 轻量任务、频繁调用、成本敏感
- `sonnet` — 标准开发、代码审查、大多数任务
- `opus` — 架构决策、复杂推理、规划

**工具权限模式：**
- 审查类代理（只读）：`["Read", "Grep", "Glob", "Bash"]`
- 开发类代理（读写）：`["Read", "Write", "Edit", "Bash", "Grep", "Glob"]`
- 架构类代理（分析）：`["Read", "Grep", "Glob"]`

## Settings 文件格式

```json
{
  "permissions": {
    "allowedTools": [
      "Read(*)",
      "Write(*)",
      "Edit(*)",
      "Glob(*)",
      "Grep(*)",
      "Bash(具体命令 *)"
    ]
  }
}
```

## Hooks 文件格式

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "shell 命令",
            "timeout": 15
          }
        ],
        "description": "此钩子的功能说明",
        "id": "pre:bash:action-name"
      }
    ],
    "PostToolUse": [],
    "Stop": []
  }
}
```

## Command 文件格式

```markdown
---
description: 调用此命令时执行的操作
---

# 命令名称

## 功能
分步描述。

## 何时使用
调用此命令的条件。
```

## CLAUDE.md 结构

```markdown
# 项目标题

> 一行描述

## 项目定位
本项目做什么。

## 路由规则
| 用户意图 | 路由到 | 说明 |
|---|---|---|

## 工作流
开发流程图和分步说明。

## 质量门禁
验证检查项。

## 错误处理
已知错误及解决方案。
```

## Skill 文件格式（SKILL.md）

```markdown
---
name: skill-name
description: 何时使用以及包含什么内容
version: 1.0.0
---

# 技能标题

## 内容
参考知识和最佳实践。
```
