
# PydanticAI 上下文工程 —— AI 智能体开发全局规范

本文件包含适用于所有 PydanticAI 智能体开发的全局规则和原则。这些规则专为构建具备工具、记忆和结构化输出的生产级 AI 智能体而设。

## 🔄 PydanticAI 核心原则

**重要：以下原则适用于所有 PydanticAI 智能体开发：**

### 智能体开发工作流
- **始终从 INITIAL.md 开始** —— 在生成 PRP 前先定义智能体需求
- **采用 PRP 流程**：INITIAL.md → `/generate-pydantic-ai-prp INITIAL.md` → `/execute-pydantic-ai-prp PRPs/filename.md`
- **遵循验证循环** —— 每个 PRP 必须包含 TestModel/FunctionModel 的智能体测试
- **上下文为王** —— 必须包含所有必要的 PydanticAI 模式、示例和文档


### 智能体调研方法论
- **广泛网络检索** —— 持续调研 PydanticAI 模式与最佳实践
- **研读官方文档** —— ai.pydantic.dev 是权威资料来源
- **模式提炼** —— 识别可复用的智能体架构和工具模式
- **记录易错点** —— 文档化异步模式、模型限制和上下文管理问题


## 📚 项目意识与上下文

- **始终使用虚拟环境** 运行所有代码和测试，如项目中未包含请自行创建
- **统一 PydanticAI 命名规范** 和智能体结构模式
- **遵循既定的智能体目录组织**（如 agent.py、tools.py、models.py）
- **充分参考 PydanticAI 示例** —— 在创建新智能体前先学习现有模式


## 🧱 智能体结构与模块化

- **单文件不超过 500 行** —— 接近上限时请拆分为多个模块
- **按职责清晰分模块组织代码**：
  - `agent.py` —— 智能体主定义与执行逻辑
  - `tools.py` —— 智能体用到的工具函数
  - `models.py` —— Pydantic 输出模型与依赖类
  - `dependencies.py` —— 上下文依赖与外部服务集成
- **保持清晰一致的 import** —— 正确从 pydantic_ai 包导入
- **环境变量用 python-dotenv 和 load_dotenv()** —— 参考 examples/main_agent_reference/settings.py
- **敏感信息绝不硬编码** —— API key 和配置始终用 .env 文件管理

## 🤖 PydanticAI 开发规范

### 智能体创建模式
- **采用模型无关设计** —— 支持多种模型提供方（如 OpenAI、Anthropic、Gemini）
- **实现依赖注入** —— 用 deps_type 注入外部服务和上下文
- **定义结构化输出** —— 用 Pydantic 模型做结果校验
- **系统提示词要全面** —— 包含静态和动态指令

### 工具集成规范
- **用 @agent.tool 装饰器** 实现带上下文的工具（RunContext[DepsType]）
- **用 @agent.tool_plain 装饰器** 实现无依赖的简单工具
- **参数校验要规范** —— 工具参数用 Pydantic 模型
- **优雅处理工具错误** —— 实现重试和错误恢复机制

### 环境变量配置（python-dotenv）
```python
# Use python-dotenv and pydantic-settings for proper configuration management
from pydantic_settings import BaseSettings
from pydantic import Field, ConfigDict
from dotenv import load_dotenv
from pydantic_ai.providers.openai import OpenAIProvider
from pydantic_ai.models.openai import OpenAIModel

class Settings(BaseSettings):
    """Application settings with environment variable support."""
    
    model_config = ConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        case_sensitive=False,
        extra="ignore"
    )
    
    # LLM Configuration
    llm_provider: str = Field(default="openai", description="LLM provider")
    llm_api_key: str = Field(..., description="API key for the LLM provider")
    llm_model: str = Field(default="gpt-4", description="Model name to use")
    llm_base_url: str = Field(
        default="https://api.openai.com/v1", 
        description="Base URL for the LLM API"
    )

def load_settings() -> Settings:
    """Load settings with proper error handling and environment loading."""
    # Load environment variables from .env file
    load_dotenv()
    
    try:
        return Settings()
    except Exception as e:
        error_msg = f"Failed to load settings: {e}"
        if "llm_api_key" in str(e).lower():
            error_msg += "\nMake sure to set LLM_API_KEY in your .env file"
        raise ValueError(error_msg) from e

def get_llm_model():
    """Get configured LLM model with proper environment loading."""
    settings = load_settings()
    provider = OpenAIProvider(
        base_url=settings.llm_base_url, 
        api_key=settings.llm_api_key
    )
    return OpenAIModel(settings.llm_model, provider=provider)
```


### 智能体测试规范
- **开发阶段用 TestModel** —— 快速校验，无需 API 调用
- **自定义行为用 FunctionModel** —— 测试中可控智能体响应
- **用 Agent.override() 做测试隔离** —— 测试上下文中替换模型
- **同步/异步模式都要测** —— 保证不同执行方式兼容
- **工具参数和错误校验要测** —— 校验工具参数 schema 和错误处理


## ✅ 智能体开发任务管理

- **将开发拆解为明确步骤** —— 每步有具体完成标准
- **实现后立即标记完成** —— 任务完成及时更新
- **开发进度实时同步** —— 随开发推进更新任务状态
- **实现前务必测试行为** —— 通过测试再标记任务完成


## 📎 PydanticAI 编码规范

### 智能体架构示例
```python
# 遵循 main_agent_reference 模式 —— 没有结构化输出需求时默认字符串输出
from pydantic_ai import Agent, RunContext
from dataclasses import dataclass
from .settings import load_settings

@dataclass
class AgentDependencies:
    """智能体执行依赖"""
    api_key: str
    session_id: str = None

# 正确加载 dotenv 环境变量
settings = load_settings()

# 默认字符串输出的简单智能体
agent = Agent(
    get_llm_model(),  # 内部调用 load_settings()
    deps_type=AgentDependencies,
    system_prompt="You are a helpful assistant..."
)

@agent.tool
async def example_tool(
    ctx: RunContext[AgentDependencies], 
    query: str
) -> str:
    """具备上下文访问的工具函数"""
    return await external_api_call(ctx.deps.api_key, query)
```


### 安全最佳实践
- **API key 管理** —— 用 python-dotenv 和 .env 文件，密钥绝不提交到版本库
- **环境变量加载** —— 始终用 load_dotenv()，参考 main_agent_reference/settings.py
- **输入校验** —— 所有工具参数用 Pydantic 模型
- **限流** —— 外部 API 要实现请求限流
- **防止提示注入** —— 校验和清洗用户输入
- **错误处理** —— 错误信息绝不暴露敏感内容

### 常见 PydanticAI 易错点
- **异步/同步混用问题** —— 全程保持 async/await 风格一致
- **模型 token 限制** —— 不同模型上下文长度不同，需提前规划
- **依赖注入复杂度** —— 依赖关系要简单、类型清晰
- **工具错误处理缺失** —— 必须实现重试和降级机制
- **上下文状态管理** —— 能无状态就无状态，提升可靠性


## 🔍 智能体调研规范

- **用 Archon MCP server** —— 通过 RAG 获取 PydanticAI 文档
- **研读官方示例** —— ai.pydantic.dev/examples 有完整实现
- **调研模型能力** —— 理解不同提供方的特性和限制
- **记录集成模式** —— 包含外部服务集成示例

## 🎯 智能体实现规范

- **严格遵循 PRP 工作流** —— 不可跳过智能体验证步骤
- **始终先用 TestModel 测试** —— 逻辑通过后再用真实模型
- **优先用现有智能体模式** —— 不要重复造轮子
- **工具和模型错误要全面处理**
- **流式交互要测试** —— 实现实时智能体时需覆盖

## 🚫 坚决避免的反模式

- ❌ 不做测试 —— 必须用 TestModel/FunctionModel 验证
- ❌ 模型字符串硬编码 —— 用环境变量配置，参考 main_agent_reference
- ❌ 非必要不加 result_type —— 默认字符串输出
- ❌ 忽略异步模式 —— PydanticAI 有专门的 async/sync 规范
- ❌ 依赖关系复杂 —— 保持依赖简单、可测
- ❌ 工具错误处理缺失 —— 必须有重试和降级
- ❌ 输入校验缺失 —— 所有外部输入都用 Pydantic 校验

## 🔧 智能体开发工具使用规范

- **广泛网络检索** —— 查找 PydanticAI 资料和文档
- **遵循 PydanticAI 命令模式** —— 斜杠命令和智能体工作流
- **用验证循环保障质量** —— 每步开发都要验证
- **多模型提供方测试** —— 保证智能体兼容性

## 🧪 智能体测试与可靠性

- **必须写全面测试** —— 工具、输出、错误都要覆盖
- **先用 TestModel 测试行为** —— 再用真实模型
- **覆盖边界场景** —— 工具失败、模型异常等
- **结构化/非结构化输出都要测** —— 保证灵活性
- **依赖注入要可测** —— 测试环境下依赖注入要正常

以上全局规范专为 PydanticAI 智能体开发制定，确保生产级 AI 应用具备完善的错误处理、测试和安全实践。