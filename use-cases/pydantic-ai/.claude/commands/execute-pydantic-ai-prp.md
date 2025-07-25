# 执行 Pydantic AI Agent PRP

使用 PRP 文件实现一个 Pydantic AI agent。

## PRP 文件：$ARGUMENTS

## 执行流程

1. **加载 PRP**
   - 读取指定的 Pydantic AI PRP 文件
   - 理解所有 agent 要求和调研结论
   - 遵循 PRP 中的所有指令，并在需要时扩展调研
   - 参考 main_agent_reference 的实现模式
   - 必要时进一步检索 Pydantic AI 文档和网络资料

2. **ULTRATHINK 深度思考**
   - 在执行 agent 实现计划前深入思考
   - 使用 todos 工具将 agent 开发拆解为更小的步骤
   - 使用 TodoWrite 工具创建并跟踪 agent 实现计划
   - 参考 main_agent_reference 的配置和结构
   - 规划 agent.py、tools.py、dependencies.py 及测试方案

3. **执行计划**
   - 按照 PRP 实现 Pydantic AI agent
   - 创建基于环境变量配置的 agent（settings.py, providers.py）
   - 默认使用字符串输出（除非需要结构化输出时才用 result_type）
   - 用 @agent.tool 装饰器和适当的错误处理实现工具
   - 添加包含 TestModel 和 FunctionModel 的全面测试

4. **验证**
   - 测试 agent 的导入和实例化
   - 运行 TestModel 验证以加速开发测试
   - 测试工具注册和功能
   - 如有创建，运行 pytest 测试套件
   - 验证 agent 是否遵循 main_agent_reference 模式

5. **完成**
   - 确保所有 PRP 检查项已完成
   - 用示例查询测试 agent
   - 检查安全模式（环境变量、错误处理）
   - 报告完成状态
   - 再次阅读 PRP，确保实现完整

6. **参考 PRP**
   - 如有需要，可随时再次参考 PRP

## Pydantic AI 专用实现模式

- **配置**：采用类似 main_agent_reference 的基于环境变量的配置
- **输出**：默认字符串输出，仅在需要验证时使用 result_type
- **工具**：用 @agent.tool 和 RunContext 进行依赖注入
- **测试**：开发阶段包含 TestModel 验证
- **安全**：API key 用环境变量，注意错误处理

注意：如验证失败，按 PRP 中的错误处理模式修复并重试。实现时始终参考 main_agent_reference 的最佳实践。