# 生成质量标准

## 输出质量规则

harness-expert 生成的所有文件必须遵循以下标准。

## 文件格式规则

### JSON 文件（settings.json、hooks.json）
- 有效 JSON — 必须能被 `JSON.parse()` 解析
- 2 空格缩进
- 无尾逗号
- 无注释（JSON 不支持注释）
- 以换行符结尾

### Markdown 文件（agents、commands、rules、CLAUDE.md）
- UTF-8 编码
- LF 换行符（非 CRLF）
- 以换行符结尾
- 行尾无多余空格

### Agent 文件（带 YAML frontmatter 的 .md）
- frontmatter 以独立的 `---` 行分隔
- 必填字段：`name`、`description`、`tools`、`model`
- `tools` 为 JSON 字符串数组
- `model` 为以下之一：`haiku`、`sonnet`、`opus`
- 正文以 `# 标题` 作为第一个标题

### Command 文件（带 YAML frontmatter 的 .md）
- frontmatter 以独立的 `---` 行分隔
- 必填字段：`description`
- 可选字段：`name`、`argument-hint`

### Skill 文件（SKILL.md）
- frontmatter 以独立的 `---` 行分隔
- 必填字段：`name`、`description`
- 可选字段：`origin`、`version`

## 内容规则

### CLAUDE.md
- 必须有路由表，且与所有生成的 agents 和 commands 对应
- 必须有工作流章节描述项目的开发流程
- 必须引用生成的工作空间中实际存在的文件路径
- 无断裂的交叉引用

### 代理（Agents）
- 描述必须说明**何时使用**该代理（不仅仅是它做什么）
- tools 数组必须最小化 — 只授予代理实际需要的工具
- 审查类代理：Read、Grep、Glob、Bash（禁止 Write/Edit）
- 开发类代理：Read、Write、Edit、Bash、Grep、Glob
- 模型选择：sonnet 用于大多数代理，haiku 用于轻量/高频任务，opus 用于架构决策

### 设置权限（Settings Permissions）
- TypeScript 项目：包含 `Bash(npm *)`、`Bash(npx *)`、`Bash(node *)`
- C 项目：包含 `Bash(make *)`、`Bash(cmake *)`、`Bash(gcc *)`、`Bash(clang *)`
- 永远不包含不受限的 `Bash(*)` — 始终限定到具体命令模式
- 始终包含：`Read(*)`、`Write(*)`、`Edit(*)`、`Glob(*)`、`Grep(*)`

### 钩子（Hooks）
- 每个 hook 必须有唯一的 `id`，遵循 `phase:tool:action` 约定
- 每个 hook 必须有 `description` 说明其作用
- 默认超时：PreToolUse 15 秒，PostToolUse 30 秒，Stop 300 秒
- 异步 hook 仅用于非阻塞分析（永远不用于需要阻塞的验证）

## 命名规则

- 代理名称：`kebab-case`，描述性角色名（如 `api-developer`、`code-reviewer`）
- 命令名称：`kebab-case`，动词或名词（如 `build`、`test`、`dev`）
- Hook ID：`phase:tool:action`（如 `pre:bash:block-dangerous`）
- 模板变量：`{{UPPER_SNAKE_CASE}}`
- 文件名：markdown 用 `kebab-case.md`，JSON 用 `kebab-case.json`

## 验证清单

生成工作空间后，验证以下各项：

- [ ] 所有 JSON 文件解析成功
- [ ] 所有 agent 文件有有效的 YAML frontmatter 且包含必填字段
- [ ] CLAUDE.md 路由表仅引用实际存在的 agents/commands
- [ ] settings.json 权限与所选语言匹配
- [ ] hooks.json 中的 hook ID 唯一
- [ ] 无空文件（每个文件都有有意义的内容）
- [ ] 目录结构与预期布局一致
