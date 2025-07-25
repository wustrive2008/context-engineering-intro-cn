## 功能描述（FEATURE）:

[请用你自己的内容替换所有中括号部分]
[简要概述你要构建的智能体，越详细越好！]
[示例：构建一个简单的调研智能体，能用 Pydantic AI 结合 Brave API 进行主题调研，并用 Gmail 草稿邮件分享洞见。]

## 工具（TOOLS）:

[描述你希望智能体具备的工具——功能、参数、返回值等，越具体越好。]

## 依赖（DEPENDENCIES）

[描述智能体工具所需的依赖（Pydantic AI RunContext），如 API key、数据库连接、HTTP 客户端等。]

## 系统提示词（SYSTEM PROMPT(S)）

[描述智能体的系统指令——可以直接写完整提示词，也可以给出指导性描述。]

## 示例（EXAMPLES）:

[如有更多示例智能体/工具实现，可放在 examples/ 文件夹并在此引用。]
[本模板已包含以下 Pydantic AI 示例：]

- examples/basic_chat_agent —— 带对话记忆的基础聊天智能体
- examples/tool_enabled_agent —— 支持网页搜索的工具增强型智能体
- examples/structured_output_agent —— 用于数据校验的结构化输出智能体
- examples/testing_examples —— 包含 TestModel 和 FunctionModel 的测试示例
- examples/main_agent_reference —— Pydantic AI 智能体最佳实践

## 文档（DOCUMENTATION）:

[如需引用更多文档，可放在 PRPs/ai_docs 或添加 URL。]

- Pydantic AI 官方文档：https://ai.pydantic.dev/
- 智能体创建指南：https://ai.pydantic.dev/agents/
- 工具集成文档：https://ai.pydantic.dev/tools/
- 测试模式文档：https://ai.pydantic.dev/testing/
- 模型提供方文档：https://ai.pydantic.dev/models/

## 其他注意事项（OTHER CONSIDERATIONS）:

- API key 配置请用环境变量，禁止硬编码模型字符串
- 智能体尽量保持简单——默认字符串输出，除非确实需要结构化输出
- 配置和模型提供方请遵循 main_agent_reference 模式
- 开发阶段务必用 TestModel 做全面测试

[如有其他注意事项或“坑”，请在此补充，便于编程助手规避问题。]