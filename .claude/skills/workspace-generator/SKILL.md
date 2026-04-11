---
name: workspace-generator
description: 生成 Claude Code harness 工作空间的参考材料和最佳实践。提供格式规范和生成原则。由 harness-expert 代理在生成过程中参考。
version: 2.0.0
---

# 工作空间生成器 — 参考资料与最佳实践

## Harness 架构概览

Claude Code harness 工作空间是一个 `.claude/` 目录结构，用于配置 Claude Code
在项目中的运行方式。它由 7 种组件类型组成：

| 组件 | 文件/路径 | 用途 |
|---|---|---|
| **CLAUDE.md** | `<project>/CLAUDE.md` | 项目编排器 — 路由、工作流、质量门禁 |
| **设置** | `.claude/settings.json` | 工具权限和环境配置 |
| **代理** | `.claude/agents/*.md` | 专注特定角色的子代理 |
| **技能** | `.claude/skills/*/SKILL.md` | 可复用的知识和参考材料 |
| **规则** | `.claude/rules/*.md` | 编码标准和约定 |
| **命令** | `.claude/commands/*.md` | 用户调用的斜杠命令 |
| **钩子** | `.claude/hooks/hooks.json` | 工具使用前后的自动化动作 |

## 格式规范

详细格式参见 `templates/_base/format-reference.md`。核心要点：

### Agent 文件
```yaml
---
name: kebab-case-name
description: 何时使用 + 做什么
tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]
model: sonnet
---
```
- 审查类代理（只读）：tools 不含 Write/Edit
- 开发类代理（读写）：tools 含全部
- 架构类代理（分析）：tools 仅 Read/Grep/Glob

### Settings 文件
- 权限始终限定到具体命令模式，永远不用 `Bash(*)`
- 必含 `Read(*)`、`Write(*)`、`Edit(*)`、`Glob(*)`、`Grep(*)`

### Hooks 文件
- 每个 hook 有唯一 `id`（`phase:tool:action` 格式）
- 三级自动化：minimal（无 hooks）、standard（阻止危险命令+构建检查）、strict（+自动 lint+测试检查）

### Command 文件
```yaml
---
description: 功能描述
---
```

## 自动化级别参考

### 最小（minimal）
- 无 hooks
- 基础权限

### 标准（standard）
- **PreToolUse：** 阻止危险 Bash 命令（`rm -rf /`、`sudo`、`chmod 777`、fork bomb）
- **Stop：** 构建检查

### 严格（strict）
- 包含 standard 全部，另外：
- **PostToolUse：** 写入/编辑时自动 lint 和格式化
- **Stop：** 构建检查 + 测试检查 + 覆盖率报告
- **PreToolUse：** 还阻止 `--no-verify` 等绕过标志

## 生成原则

### 应该做
- 权限限定到具体命令 — 永远不用 `Bash(*)`
- 代理描述说明**何时使用**
- **最后**生成 CLAUDE.md
- 生成后验证所有 JSON
- 代理和命令完全从问答中产生，不套用预设模板
- 每次都是全新设计

### 不应该做
- 生成空文件
- 给审查类代理 Write/Edit 权限
- 读取目标路径或其他项目的已有配置
- 跳过验证
- 替用户做决定

## 模板变量参考

| 变量 | 说明 |
|---|---|
| `{{PROJECT_NAME}}` | kebab-case 项目标识符 |
| `{{PROJECT_DESCRIPTION}}` | 一行描述 |
| `{{LANGUAGE}}` | 编程语言（从用户技术栈中提取） |
| `{{AGENT_LIST}}` | 用户确认的代理列表 |
| `{{COMMAND_LIST}}` | 用户确认的命令列表 |
| `{{HOOKS_TIER}}` | 自动化级别 |
| `{{METHODOLOGY}}` | 开发方法论 |
| `{{WORKFLOW}}` | 工作流图 |
| `{{BUILD_CMD}}` | 构建命令（根据技术栈推断） |
| `{{TEST_CMD}}` | 测试命令（根据技术栈推断） |
| `{{LINT_CMD}}` | 检查命令（根据技术栈推断） |
| `{{FORMAT_CMD}}` | 格式化命令（根据技术栈推断） |
