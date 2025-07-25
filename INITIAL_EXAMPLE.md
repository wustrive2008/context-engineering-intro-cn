## 功能需求：

- 一个Pydantic AI Agent，其中一个Pydantic AI Agent作为工具。
- 主Agent的研究Agent和子Agent的邮件起草Agent。
- 与Agent交互的CLI。
- 邮件起草Agent使用Gmail，研究Agent使用Brave API。

## 示例：

在 `examples/` 文件夹中，有一个README供您阅读以了解示例的全部内容，以及在为上述功能创建文档时如何构建您自己的README。

- `examples/cli.py` - 用作创建CLI的模板
- `examples/agent/` - 阅读这里的所有文件，了解创建支持不同提供商和LLM的Pydantic AI Agent、处理Agent依赖关系和向Agent添加工具的最佳实践。

不要直接复制这些示例，它们完全是针对不同项目的。但请将其作为灵感和最佳实践。

## 文档：

Pydantic AI 文档：https://ai.pydantic.dev/

## 其他考虑因素：

- 包含 .env.example 文件，README中包含设置说明，包括如何配置Gmail和Brave。
- 在README中包含项目结构。
- 虚拟环境已设置必要的依赖项。
- 使用 python_dotenv 和 load_env() 处理环境变量
