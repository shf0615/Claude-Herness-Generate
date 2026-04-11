---
description: 检查并分析已有的 .claude/ 工作空间结构，报告组件清单和配置状态。
argument-hint: "<目录路径>"
---

# 预览 Harness 工作空间

## 功能

分析指定目录下的 `.claude/` 工作空间结构，报告：
- 已有组件列表（agents、commands、rules、hooks、skills）
- 配置摘要（权限、自动化级别）
- 潜在问题（缺失组件、无效 JSON、断裂引用）

## 用法

```
/preview /path/to/project
```

## 分析步骤

1. **检查 .claude/ 目录** — 确认存在并列出结构
2. **读取 settings.json** — 分析权限配置
3. **列出 agents/** — 统计代理数量和角色
4. **列出 commands/** — 统计命令数量
5. **检查 hooks.json** — 分析自动化级别
6. **读取 CLAUDE.md** — 检查路由表和工作流
7. **交叉验证** — 路由表引用的代理是否都存在

## 输出格式

```
工作空间分析报告：/path/to/project

组件:
  ├── settings.json    ✅ 有效 JSON，X 个权限规则
  ├── agents/          ✅ N 个代理
  ├── commands/        ✅ N 个命令
  ├── rules/           ✅ N 个规则文件
  ├── hooks/           ✅ 自动化级别：standard
  ├── skills/          ⚠️ 无
  └── CLAUDE.md        ✅ 有路由表

问题:
  - ⚠️ 描述

建议:
  - 💡 描述
```
