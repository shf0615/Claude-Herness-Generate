# 工作空间设计规范

## 核心原则

1. **问答优先**：每个阶段结束后 STOP，等用户确认后再继续
2. **不跨阶段**：一个回复只处理一个阶段
3. **不替用户决定**：始终提供选项，让用户做最终选择
4. **开放式输入**：不限定项目类型和技术栈，用户自由描述
5. **中文交互**：所有问答和输出使用中文

## 生成规范

### 文件生成顺序
```
1. 目录结构（mkdir -p）
2. settings.json（权限配置）
3. rules/（编码规范）
4. agents/（代理定义）
5. skills/（技能知识库）
6. commands/（命令入口）
7. hooks/hooks.json（自动化钩子）
8. docs/contracts/（契约文件）
9. CLAUDE.md（最后生成，引用所有组件）
```

### 权限规范
- 永远不授予 `Bash(*)`
- 使用具体命令模式：`Bash(npm *)`, `Bash(cargo *)` 等
- 只读 agent 不授予 Write/Edit 权限

### 契约文件格式
```markdown
# 契约：{源 agent} → {目标 agent}

## 用途
{一句话说明}

## 字段
| 字段 | 类型 | 说明 |
|------|------|------|

## 更新时机
{什么时候由谁更新}

## 读取方式
{目标 agent 如何引用此文件}
```

## Agent 设计规范

### 必须包含
- YAML frontmatter（name, description, tools, model）
- 硬性停止点定义
- 禁止行为列表
- 使用 AskUserQuestion 进行交互

### Agent 间通信
- 通过 `docs/contracts/` 目录的 markdown 文件传递上下文
- 每个契约文件明确定义源 agent、目标 agent、字段格式
- Agent prompt 中包含 Read 指令读取契约文件
