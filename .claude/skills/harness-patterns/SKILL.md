# Harness 设计模式知识库

## 契约文件模式（Contract File Pattern）

### 概述
Agent 间通过 `docs/contracts/` 目录的结构化 markdown 文件传递上下文，实现松耦合的协作。

### 常见契约类型

| 契约 | 源 → 目标 | 内容 |
|------|-----------|------|
| 架构契约 | architect → developer | 系统架构、模块划分、API 接口定义 |
| API 契约 | backend → frontend | 端点定义、请求/响应格式、状态码 |
| 实现契约 | developer → reviewer | 实现说明、已知限制、关注点 |
| 审查契约 | reviewer → developer | 审查结果、修复建议、优先级 |
| 测试契约 | developer → tester | 测试范围、边界条件、预期行为 |

### 设计原则
1. **单一职责**：每个契约文件只描述一个方向的通信
2. **明确所有权**：只有源 agent 可以写入，目标 agent 只读
3. **版本化**：契约文件包含更新时间戳
4. **可验证**：目标 agent 可以检查契约是否过期

## Agent 角色模式

### 开发者类（可写）
- 拥有 Read/Write/Edit/Bash 权限
- 负责代码生成和修改
- 模型：sonnet（复杂任务）或 haiku（简单任务）

### 审查者类（只读）
- 仅拥有 Read/Glob/Grep 权限
- 负责代码审查、安全检查
- 不能修改代码，只能提出建议

### 编排者类（协调）
- 拥有 Read 权限 + Agent 调度能力
- 负责工作流编排、任务分配
- 模型：sonnet 或 opus（需要深度推理）

## 工作流模式

### 线性流水线
```
developer → tester → reviewer → 提交
```
适用于：简单项目、小团队

### 并行分支
```
architect → [frontend-dev, backend-dev] → integrator → reviewer
```
适用于：全栈项目、前后端分离

### 迭代循环
```
developer ⇄ reviewer（循环直到通过）→ 提交
```
适用于：高质量要求的项目

## 自动化级别

### minimal（最小）
- 无 hooks
- 手动控制所有流程

### standard（标准，推荐）
- PreToolUse: 阻止危险操作（`rm -rf /`, `git push --force`）
- Stop: 结束时检查构建状态

### strict（严格）
- 包含 standard 的所有 hooks
- PostToolUse: 文件保存后自动 lint/format
- Stop: 构建 + 测试 + 覆盖率检查
