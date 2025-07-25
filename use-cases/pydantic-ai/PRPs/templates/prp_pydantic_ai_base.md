---
name: "PydanticAI 智能体 PRP 模板"
description: "用于生成 PydanticAI 智能体开发项目的全流程 PRP 中文模板"

## 目标说明（Purpose）

[简要描述要构建的 PydanticAI 智能体及其主要用途]

## 核心原则（Core Principles）

1. **PydanticAI 最佳实践**：深度集成 PydanticAI 智能体创建、工具、结构化输出等模式
2. **面向生产**：实现安全、测试和监控，适用于生产环境
3. **类型安全优先**：全程利用 PydanticAI 的类型安全设计和校验
4. **上下文工程集成**：将成熟的上下文工程工作流应用于智能体开发
5. **全面测试**：开发和验证阶段均用 TestModel 和 FunctionModel


## ⚠️ 实现指南：切勿过度设计

**重要提示**：让你的智能体实现聚焦且实用，避免不必要的复杂度。

### 不要做的事：
- ❌ **不要造几十个工具** —— 只实现智能体真正需要的工具
- ❌ **不要让依赖注入过于复杂** —— 保持依赖注入简单聚焦
- ❌ **不要加无用抽象** —— 直接遵循 main_agent_reference 模式
- ❌ **除非确有需求，不要构建复杂工作流**
- ❌ **除非确需校验，不要加结构化输出**（默认字符串输出）
- ❌ **不要在 examples/ 目录下开发**

### 推荐做法：
- ✅ **从简单开始** —— 先实现满足需求的最小可用智能体
- ✅ **按需递增工具** —— 只实现智能体实际需要的功能
- ✅ **遵循 main_agent_reference** —— 用成熟模式，不要重复造轮子
- ✅ **默认字符串输出** —— 只有确需校验时才加 result_type
- ✅ **早测多测** —— 开发中用 TestModel 持续验证

### 关键自问：
**“这个功能对实现智能体核心目标真的必要吗？”**

如果答案是否定的，就不要做。保持简单、聚焦、实用。

---


## 目标（Goal）

[详细描述该智能体应实现的目标]

## 背景与动因（Why）

[说明为什么需要这个智能体，以及它要解决什么问题]

## 需求细节（What）


### 智能体类型分类
- [ ] **对话智能体**：具备记忆和上下文的会话接口
- [ ] **工具增强型智能体**：可集成外部工具的智能体
- [ ] **工作流智能体**：多步骤任务处理与编排
- [ ] **结构化输出智能体**：复杂数据校验与格式化

### 模型提供方要求
- [ ] **OpenAI**：`openai:gpt-4o` 或 `openai:gpt-4o-mini`
- [ ] **Anthropic**：`anthropic:claude-3-5-sonnet-20241022` 或 `anthropic:claude-3-5-haiku-20241022`
- [ ] **Google**：`gemini-1.5-flash` 或 `gemini-1.5-pro`
- [ ] **兜底策略**：多提供方支持，自动故障切换

### 外部集成
- [ ] 数据库连接（请注明类型，如 PostgreSQL、MongoDB 等）
- [ ] REST API 集成（列出所需服务）
- [ ] 文件系统操作
- [ ] 网页抓取或搜索能力
- [ ] 实时数据源


### 成功标准（Success Criteria）
- [ ] 智能体能成功处理所有指定用例
- [ ] 所有工具均能正确运行并具备错误处理
- [ ] 结构化输出能通过 Pydantic 模型校验
- [ ] 测试覆盖全面，包含 TestModel 和 FunctionModel
- [ ] 安全措施到位（API key、输入校验、限流等）
- [ ] 性能满足需求（响应时间、吞吐量等）

## 所有所需上下文（All Needed Context）

### PydanticAI 文档与调研（Documentation & Research）

```yaml
# MCP 服务器
- mcp: Archon
  query: "PydanticAI 智能体创建、模型提供方、工具、依赖"
  why: 核心框架理解与最新模式调研

# 关键 Pydantic AI 文档（必须调研）
- url: https://ai.pydantic.dev/
  why: 官方 PydanticAI 文档与入门指南
  content: 智能体创建、模型提供方、依赖注入模式

- url: https://ai.pydantic.dev/agents/
  why: 全面智能体架构与配置模式
  content: 系统提示词、输出类型、执行方式、智能体组合

- url: https://ai.pydantic.dev/tools/
  why: 工具集成模式与函数注册
  content: @agent.tool 装饰器、RunContext 用法、参数校验

- url: https://ai.pydantic.dev/testing/
  why: PydanticAI 智能体专用测试策略
  content: TestModel、FunctionModel、Agent.override()、pytest 模式

- url: https://ai.pydantic.dev/models/
  why: 模型提供方配置与认证
  content: OpenAI、Anthropic、Gemini 配置、API key 管理、兜底模型

# 内置示例
- path: examples/
  why: Pydantic AI 智能体参考实现
  content: 多个已实现的简单 Pydantic AI 示例，包含模型与提供方配置

- path: examples/cli.py
  why: 展示 Pydantic AI 智能体的真实交互
  content: 支持流式、工具调用可见、对话管理的 CLI，演示用户实际如何与智能体交互
```


### 智能体架构调研（Agent Architecture Research）

```yaml
# PydanticAI 架构模式（遵循 main_agent_reference）
agent_structure:
  configuration:
    - settings.py：基于 pydantic-settings 的环境变量配置
    - providers.py：模型提供方抽象，get_llm_model()
    - API key 和模型选择用环境变量
    - 切勿硬编码模型字符串如 "openai:gpt-4o"
  
  agent_definition:
    - 默认字符串输出（无结构化需求时不加 result_type）
    - 模型配置用 providers.py 的 get_llm_model()
    - 系统提示词用字符串常量或函数
    - 外部服务依赖用 dataclass
  
  tool_integration:
    - @agent.tool：带上下文的工具（RunContext[DepsType]）
    - 工具函数应为纯函数，可独立调用
    - 工具实现需有错误处理和日志
    - 依赖注入通过 RunContext.deps
  
  testing_strategy:
    - TestModel：快速开发验证
    - FunctionModel：自定义行为测试
    - Agent.override()：测试隔离
    - 工具测试要有 mock
```


### 安全与生产环境注意事项（Security and Production Considerations）

```yaml
# PydanticAI 安全模式（需调研）
security_requirements:
  api_management:
    environment_variables: ["OPENAI_API_KEY", "ANTHROPIC_API_KEY", "GEMINI_API_KEY"]
    secure_storage: "API key 绝不提交到版本库"
    rotation_strategy: "规划密钥轮换与管理"
  
  input_validation:
    sanitization: "所有用户输入用 Pydantic 模型校验"
    prompt_injection: "实现提示注入防护策略"
    rate_limiting: "合理限流防止滥用"
  
  output_security:
    data_filtering: "确保响应中无敏感数据"
    content_validation: "校验输出结构和内容"
    logging_safety: "日志安全，绝不暴露密钥"
```


### 常见 PydanticAI 易错点（Common Gotchas，需调研并记录）

```yaml
# 智能体常见易错点（需调研并规避）
implementation_gotchas:
  async_patterns:
    issue: "同步/异步调用混用导致问题"
    research: "PydanticAI async/await 最佳实践"
    solution: "[调研后补充]"
  
  model_limits:
    issue: "不同模型能力和 token 限制不同"
    research: "模型提供方能力对比"
    solution: "[调研后补充]"
  
  dependency_complexity:
    issue: "依赖关系复杂难以调试"
    research: "PydanticAI 依赖注入最佳实践"
    solution: "[调研后补充]"
  
  tool_error_handling:
    issue: "工具失败会导致整个智能体崩溃"
    research: "工具错误处理与重试模式"
    solution: "[调研后补充]"
```


## 实现蓝图（Implementation Blueprint）

### 技术调研阶段（Technology Research Phase）

**调研必做——正式开发前必须完成：**

✅ **PydanticAI 框架深度学习：**
- [ ] 智能体创建模式与最佳实践
- [ ] 模型提供方配置与兜底策略
- [ ] 工具集成模式（@agent.tool vs @agent.tool_plain）
- [ ] 依赖注入体系与类型安全
- [ ] 测试策略（TestModel、FunctionModel）

✅ **智能体架构调研：**
- [ ] 项目结构规范（agent.py、tools.py、models.py、dependencies.py）
- [ ] 系统提示词设计（静态/动态）
- [ ] Pydantic 模型结构化输出校验
- [ ] 异步/同步模式与流式支持
- [ ] 错误处理与重试机制

✅ **安全与生产模式：**
- [ ] API key 管理与安全配置
- [ ] 输入校验与提示注入防护
- [ ] 限流与监控策略
- [ ] 日志与可观测性
- [ ] 部署与扩展性考量


### 智能体实现计划（Agent Implementation Plan）

```yaml
实现任务 1 - 智能体架构搭建（遵循 main_agent_reference 模式）：
  创建智能体项目结构：
    - settings.py：基于 pydantic-settings 的环境变量配置
    - providers.py：模型提供方抽象，get_llm_model()
    - agent.py：主智能体定义（默认字符串输出）
    - tools.py：工具函数，带正确装饰器
    - dependencies.py：外部服务集成（dataclass）
    - tests/：全面测试用例

实现任务 2 - 核心智能体开发：
  按 main_agent_reference 模式实现 agent.py：
    - 使用 providers.py 的 get_llm_model() 进行模型配置
    - 系统提示词用字符串常量或函数
    - 依赖注入用 dataclass
    - 除非确需结构化输出，否则不加 result_type
    - 实现错误处理与日志

实现任务 3 - 工具集成：
  开发 tools.py：
    - 工具函数用 @agent.tool 装饰器注册
    - 集成 RunContext[DepsType] 以访问依赖
    - 参数校验用类型注解
    - 实现错误处理与重试机制
    - 工具文档与 schema 自动生成

实现任务 4 - 数据模型与依赖：
  创建 models.py 和 dependencies.py：
    - 用 Pydantic 定义结构化输出模型
    - 外部服务依赖类
    - 工具输入校验模型
    - 自定义校验器与约束

实现任务 5 - 全面测试：
  实现测试用例：
    - 集成 TestModel，支持快速开发验证
    - FunctionModel 测试自定义行为
    - Agent.override() 用于测试隔离
    - 与真实模型提供方的集成测试
    - 工具校验与异常场景测试

实现任务 6 - 安全与配置：
  配置安全模式：
    - API key 用环境变量管理
    - 输入清洗与校验
    - 实现限流
    - 日志与监控安全
    - 生产部署配置
```


## 验证循环（Validation Loop）

### 第一层：智能体结构验证

```bash
# Verify complete agent project structure
find agent_project -name "*.py" | sort
test -f agent_project/agent.py && echo "Agent definition present"
test -f agent_project/tools.py && echo "Tools module present"
test -f agent_project/models.py && echo "Models module present"
test -f agent_project/dependencies.py && echo "Dependencies module present"

# Verify proper PydanticAI imports
grep -q "from pydantic_ai import Agent" agent_project/agent.py
grep -q "@agent.tool" agent_project/tools.py
grep -q "from pydantic import BaseModel" agent_project/models.py

# Expected: All required files with proper PydanticAI patterns
# If missing: Generate missing components with correct patterns
```


### 第二层：智能体功能验证

```bash
# Test agent can be imported and instantiated
python -c "
from agent_project.agent import agent
print('Agent created successfully')
print(f'Model: {agent.model}')
print(f'Tools: {len(agent.tools)}')
"

# Test with TestModel for validation
python -c "
from pydantic_ai.models.test import TestModel
from agent_project.agent import agent
test_model = TestModel()
with agent.override(model=test_model):
    result = agent.run_sync('Test message')
    print(f'Agent response: {result.output}')
"

# Expected: Agent instantiation works, tools registered, TestModel validation passes
# If failing: Debug agent configuration and tool registration
```


### 第三层：全面测试验证

```bash
# Run complete test suite
cd agent_project
python -m pytest tests/ -v

# Test specific agent behavior
python -m pytest tests/test_agent.py::test_agent_response -v
python -m pytest tests/test_tools.py::test_tool_validation -v
python -m pytest tests/test_models.py::test_output_validation -v

# Expected: All tests pass, comprehensive coverage achieved
# If failing: Fix implementation based on test failures
```


### 第四层：生产就绪度验证

```bash
# Verify security patterns
grep -r "API_KEY" agent_project/ | grep -v ".py:" # Should not expose keys
test -f agent_project/.env.example && echo "Environment template present"

# Check error handling
grep -r "try:" agent_project/ | wc -l  # Should have error handling
grep -r "except" agent_project/ | wc -l  # Should have exception handling

# Verify logging setup
grep -r "logging\|logger" agent_project/ | wc -l  # Should have logging

# Expected: Security measures in place, error handling comprehensive, logging configured
# If issues: Implement missing security and production patterns
```


## 最终验证清单（Final Validation Checklist）

### 智能体实现完整性

- [ ] 智能体项目结构完整：包含 `agent.py`、`tools.py`、`models.py`、`dependencies.py`
- [ ] 智能体实例化时正确配置模型提供方
- [ ] 工具通过 @agent.tool 装饰器注册，并集成 RunContext 上下文
- [ ] 结构化输出通过 Pydantic 模型校验
- [ ] 依赖注入配置合理并经过测试
- [ ] 测试用例全面，覆盖 TestModel 和 FunctionModel


### PydanticAI 最佳实践

- [ ] 全程类型安全，使用正确的类型注解和校验
- [ ] 实现安全模式（API 密钥、输入校验、限流）
- [ ] 健全的错误处理与重试机制，保证健壮性
- [ ] 异步/同步模式统一且合理
- [ ] 完善的文档与代码注释，便于维护



### 生产就绪度

- [ ] 环境配置采用 .env 文件并进行校验
- [ ] 日志与监控配置完善，具备可观测性
- [ ] 性能优化与资源管理到位
- [ ] 部署前配置管理规范，确保上线可用
- [ ] 维护与更新策略有文档说明

---


### 反模式须避免（Anti-Patterns to Avoid）

### PydanticAI 智能体开发

- ❌ 切勿跳过 TestModel 校验 —— 开发阶段务必用 TestModel 持续验证
- ❌ 切勿硬编码 API 密钥 —— 所有凭证必须用环境变量管理
- ❌ 切勿忽视异步模式 —— PydanticAI 对异步/同步有严格要求
- ❌ 切勿构建复杂工具链 —— 工具应聚焦、可组合
- ❌ 切勿省略错误处理 —— 必须实现全面的重试与兜底机制


### 智能体架构

- ❌ 切勿混用智能体类型 —— 聊天、工具、工作流、结构化输出需明确分离
- ❌ 切勿忽视依赖注入 —— 必须用类型安全的依赖管理
- ❌ 切勿跳过输出校验 —— 结构化响应必须用 Pydantic 模型
- ❌ 切勿遗漏工具文档 —— 所有工具需有详细描述和 schema


### 安全与生产

- ❌ 切勿暴露敏感数据 —— 校验所有输出和日志，确保安全
- ❌ 切勿跳过输入校验 —— 所有用户输入需清洗和校验
- ❌ 切勿忽略限流 —— 外部服务必须有合理限流
- ❌ 切勿无监控上线 —— 部署时必须包含可观测性

**调研状态（RESEARCH STATUS）：[待补充]** —— 在正式开发前需完成 PydanticAI 全面调研。