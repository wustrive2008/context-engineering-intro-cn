# 创建 PRP

## 功能文件：$ARGUMENTS

为通用功能实现生成一份完整的 PRP，需进行充分调研。确保将上下文传递给 AI agent，使其能够自我验证并迭代完善。请先阅读功能文件，理解需要创建的内容、示例的作用及其他注意事项。

AI agent 只能获取你附加到 PRP 和训练数据中的上下文。假设 AI agent 能访问代码库且知识截止点与你一致，因此你的调研结论必须包含或在 PRP 中引用。Agent 具备 Web 搜索能力，请传递文档和示例的 URL。

## 调研流程

1. **代码库分析**
   - 检索代码库中类似的功能/模式
   - 确定需要在 PRP 中引用的文件
   - 记录需要遵循的现有约定
   - 检查测试模式以确定验证方法

2. **外部调研**
   - 在线检索类似功能/模式
   - 查阅库文档（附具体 URL）
   - 查找实现示例（GitHub/StackOverflow/博客）
   - 总结最佳实践与常见陷阱
   - 使用 Archon MCP server 获取最新 Pydantic AI 文档
   - 针对 agent 类型进行特定模式和最佳实践的网络检索
   - 调研模型提供方的能力与限制
   - 调查工具集成模式与安全注意事项
   - 记录异步/同步模式与测试策略

3. **用户澄清**（如有需要）
   - 需要参考哪些具体模式？在哪里可以找到？
   - 有哪些集成需求？在哪里可以找到？

4. **分析初始需求**
   - 阅读并理解 agent 功能需求
   - 明确所需 agent 类型（对话、工具增强、工作流、结构化输出等）
   - 确定所需模型提供方和外部集成
   - 评估 agent 实现的复杂度和范围

5. **Agent 架构规划**
   - 设计 agent 结构（agent.py、tools.py、models.py、dependencies.py）
   - 规划依赖注入模式和外部服务集成
   - 用 Pydantic 校验设计结构化输出模型
   - 规划工具注册和参数校验策略
   - 设计测试方案，采用 TestModel/FunctionModel 模式

6. **实现蓝图制定**
   - 制定详细的 agent 实现步骤
   - 规划模型提供方配置及降级策略
   - 设计工具错误处理和重试机制
   - 规划安全实现（API key、输入校验、限流）
   - 设计带有 agent 行为测试的验证循环

## PRP 生成

以 PRPs/templates/prp_pydantic_aibase.md 为模板：

### 关键上下文（需包含并传递给 AI agent）
- **文档**：带具体章节的 URL
- **代码示例**：代码库中的真实片段
- **注意事项**：库的特殊点、版本问题
- **模式**：现有可复用的实现方式

### 实现蓝图
- 以伪代码展示实现思路
- 参考真实文件中的实现模式
- 包含错误处理策略
- 按顺序列出完成 PRP 所需的全部任务

### 验证门槛（必须可执行，例如 python）
```bash
# 语法/风格检查
ruff check --fix && mypy .

# 单元测试
uv run pytest tests/ -v

```

*** 在你完成调研和代码库探索后、开始撰写 PRP 前，务必执行本步骤 ***

*** 深度思考（ULTRATHINK）PRP 并规划你的方案，然后开始撰写 PRP ***

## 输出
保存为：`PRPs/{feature-name}.md`

## 质量检查清单
- [ ] 包含所有必要上下文
- [ ] 验证门槛可被 AI 执行
- [ ] 参考了现有实现模式
- [ ] 实现路径清晰
- [ ] 错误处理有说明

请为 PRP 打分（1-10 分，表示用 claude codes 一次性实现成功的信心）

请记住：目标是通过全面上下文，一次性高质量实现。
