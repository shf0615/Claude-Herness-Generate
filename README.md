# Harness Expert

> 通过问答交互生成完整的 Claude Code harness 工作空间 — 就像 `create-react-app`，但用于 Claude Code 配置。

## 什么是 Harness Expert？

Harness Expert 是一个 Claude Code 项目级工具，通过自适应问答了解你的项目需求，
然后一键生成完整的 `.claude/` 工作空间配置，包括：

- **CLAUDE.md** — 项目编排指南（路由规则、工作流、质量门禁）
- **settings.json** — 工具权限配置
- **agents/** — 专用代理（开发者、审查员、测试员）
- **rules/** — 编码规范（按语言定制）
- **commands/** — 斜杠命令（构建、测试、开发等）
- **hooks/** — 自动化钩子

## 支持矩阵

| | TypeScript | C |
|---|---|---|
| **后端 API** | Node.js / Express / Fastify | libmicrohttpd / 嵌入式 REST |
| **前端 Web** | React / Vue / Svelte + Vite | C-to-WASM |
| **CLI / 库** | commander / npm 包 | POSIX CLI / .a .so 库 |
| **全栈** | Next.js / SvelteKit | C 后端 + WASM 前端 |

## 快速开始

### 1. 克隆项目

```bash
git clone <repo-url> harness-expert
cd harness-expert
```

### 2. 启动 Claude Code

```bash
claude
```

### 3. 运行生成命令

```
/generate
```

### 4. 回答问题

Harness Expert 会问你 4-8 个问题：

- 项目名称
- 项目类型（后端 API / 前端 Web / CLI 库 / 全栈）
- 编程语言（TypeScript / C）
- 输出路径
- _可能追问_：开发方法论、自动化级别、代理偏好

### 5. 确认并生成

预览文件结构 → 确认 → 自动生成全部文件。

```
cd /your/output/path
claude
```

## 可用命令

| 命令 | 说明 |
|---|---|
| `/generate` | 启动工作空间生成流程 |
| `/templates` | 查看所有可用项目模板 |
| `/preview <路径>` | 分析已有的工作空间 |

## 模板详情

### 后端 API（backend-api）

| 层级 | 代理 | 命令 |
|---|---|---|
| default | api-developer, api-tester | build, test |
| recommended | + api-reviewer | + dev |
| full | + security-reviewer, dba | + migrate, deploy |

### 前端 Web（frontend-web）

| 层级 | 代理 | 命令 |
|---|---|---|
| default | frontend-developer, e2e-tester | build, test |
| recommended | + component-reviewer | + dev |
| full | + accessibility-reviewer, perf-analyst | + lint, storybook |

### CLI / 库（cli-library）

| 层级 | 代理 | 命令 |
|---|---|---|
| default | cli-developer, cli-reviewer | build, test |
| recommended | 同 default | + release |
| full | + security-reviewer | + docs, benchmark |

### 全栈（fullstack）

| 层级 | 代理 | 命令 |
|---|---|---|
| default | architect, frontend-dev, backend-dev | build, test |
| recommended | + fullstack-tester, code-reviewer | + dev, status |
| full | + security-reviewer, dba, perf-analyst | + deploy, migrate |

## 自适应问答

Harness Expert 不会强制你回答 20 个问题。它根据你的回答动态决定：

- **CLI 项目 + 明确语言** → 只问 4 个核心问题就开始生成
- **全栈项目** → 追问方法论和代理偏好
- **提到了 hooks 或严格自动化** → 追问具体的自定义需求

## 自动化级别

| 级别 | PreToolUse | PostToolUse | Stop |
|---|---|---|---|
| **minimal** | 无 | 无 | 无 |
| **standard** | 阻止危险命令 | 无 | 构建检查 |
| **strict** | 阻止危险命令 + bypass 标志 | 自动 lint/format | 构建 + 测试检查 |

## 架构

```
harness-expert (Agent)          workspace-generator (Skill)
  ├── 自适应 Q&A               ├── 格式规范
  ├── 决策引擎                   ├── 最佳实践
  └── 文件生成                   └── 语言/类型映射

                    ↓ 读取模板 ↓

              templates/
  ├── _base/          (共享骨架)
  ├── backend-api/    (manifest + agents + commands)
  ├── frontend-web/   (manifest + agents + commands)
  ├── cli-library/    (manifest + agents + commands)
  └── fullstack/      (manifest + agents + commands)
```

## 项目结构

```
harness-expert/
├── CLAUDE.md                       # 项目编排器
├── README.md                       # 本文件
├── LICENSE                         # MIT
├── .claude/
│   ├── settings.json               # 工具权限
│   ├── agents/
│   │   └── harness-expert.md       # 核心 Q&A + 生成代理
│   ├── skills/
│   │   └── workspace-generator/
│   │       └── SKILL.md            # 参考材料
│   ├── rules/
│   │   └── generation-standards.md # 生成质量标准
│   ├── commands/
│   │   ├── generate.md             # /generate 命令
│   │   ├── preview.md              # /preview 命令
│   │   └── templates.md            # /templates 命令
│   └── hooks/
│       └── hooks.json              # JSON 验证钩子
└── templates/                      # 模板文件
    ├── _base/                      # 共享模板
    │   ├── claude-md.md.tmpl
    │   ├── settings.json.tmpl
    │   ├── hooks.json.tmpl
    │   └── rules/
    │       ├── typescript.md.tmpl
    │       └── c-language.md.tmpl
    ├── backend-api/
    │   ├── manifest.json
    │   ├── agents/   (3 个代理模板)
    │   └── commands/ (3 个命令模板)
    ├── frontend-web/
    │   ├── manifest.json
    │   ├── agents/   (3 个代理模板)
    │   └── commands/ (3 个命令模板)
    ├── cli-library/
    │   ├── manifest.json
    │   ├── agents/   (2 个代理模板)
    │   └── commands/ (3 个命令模板)
    └── fullstack/
        ├── manifest.json
        ├── agents/   (5 个代理模板)
        └── commands/ (4 个命令模板)
```

## 系统要求

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code)（v1.0+）
- Node.js（用于 JSON 验证）

## 扩展

### 添加新语言

1. 在 `templates/_base/rules/` 添加 `<language>.md.tmpl`
2. 在 `templates/_base/settings.json.tmpl` 添加语言权限
3. 在 `templates/_base/hooks.json.tmpl` 添加语言特定 hooks
4. 更新各模板的条件块（`{{IF_<LANGUAGE>}}`）

### 添加新项目类型

1. 在 `templates/` 下创建新目录
2. 创建 `manifest.json`（定义 3 个层级的代理和命令）
3. 创建 `agents/` 和 `commands/` 模板
4. 更新 CLAUDE.md 路由表

## 许可证

MIT
