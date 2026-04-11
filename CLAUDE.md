# Harness Expert — 交互式工作空间生成器

> 通过多轮深度问答，与用户共同设计并生成完整的 `.claude/` 工作空间配置。

## 项目定位

本项目是一个 **harness 配置专家**。用户自由描述项目需求（不限定项目类型和技术栈），
工具通过多轮问答与用户**共同设计**代理角色、命令功能、技能内容和工作流编排，
**经用户确认后**才生成完整的 `.claude/` 工作空间结构。

## 核心原则

1. **问答优先，生成在后。** 每个阶段结束后停下来等用户回复。
2. **共同设计，不套模板。** 代理、命令、工作流完全从问答中产生。
3. **每次全新设计。** 不读取目标路径或其他项目的已有配置。

## 路由规则

| 用户意图 | 路由到 | 说明 |
|---|---|---|
| 创建新工作空间 | `harness-expert` 代理 | 8 阶段问答流程（详见代理定义） |
| "generate"、"创建 harness" | `/generate` 命令 | 快捷方式，路由到代理 |
| 检查已有工作空间 | `/preview` 命令 | 分析 .claude/ 目录 |
| 咨询 harness 概念 | 直接回答 | 参考 workspace-generator 技能 |

## 工作流概要

```
阶段 1: 基础信息 → ⛔ STOP
阶段 2: 代理设计 → ⛔ STOP
阶段 3: 命令/技能/自动化 → ⛔ STOP
阶段 4: 工作流编排 → ⛔ STOP
阶段 5: 完整预览 → ⛔ STOP（确认/修改/取消）
阶段 6: 生成 → 阶段 7: 验证 → 阶段 8: 报告
```

每个 ⛔ 处代理必须停止等用户。阶段 2/3/4 的设计讨论不可跳过。
详细流程定义在 `.claude/agents/harness-expert.md` 中。

## 支持的语言模板

| 语言 | 权限模板 | 编码规范 |
|---|---|---|
| TypeScript | `settings-typescript.json` | `typescript.md.tmpl`（26 条） |
| C | `settings-c.json` | `c-language.md.tmpl`（30 条） |
| Python | `settings-python.json` | `python.md.tmpl`（32 条） |
| Go | `settings-go.json` | `go.md.tmpl`（32 条） |

其他语言由代理根据技术栈自行推断。

## 模板结构

```
templates/_base/
├── format-reference.md      格式骨架
├── claude-md.md.tmpl        CLAUDE.md 结构模板
├── settings-*.json          各语言权限模板
├── hooks-{minimal,standard,strict}.json  三级自动化
└── rules/*.md.tmpl          各语言编码规范
```

模板仅提供**格式骨架和语言规则**，代理和命令的具体内容完全从问答中产生。
