# Pydantic AI 上下文工程模板

一个用于构建生产级 Pydantic AI 智能体的综合模板，涵盖上下文工程最佳实践、工具集成、结构化输出和全面测试模式。

## 🚀 快速开始 - 复制模板

**2 分钟上手：**

```bash
# 克隆上下文工程仓库
git clone https://github.com/coleam00/Context-Engineering-Intro.git
cd Context-Engineering-Intro/use-cases/pydantic-ai

# 1. 复制本模板到你的新项目
python copy_template.py /path/to/my-agent-project

# 2. 进入你的项目目录
cd /path/to/my-agent-project

# 3. 按 PRP 工作流开始构建
# 填写 PRPs/INITIAL.md，描述你要创建的智能体

# 4. 根据详细需求生成 PRP（生成后请校验 PRP！）
/generate-pydantic-ai-prp PRPs/INITIAL.md

# 5. 执行 PRP，创建你的 Pydantic AI 智能体
/execute-pydantic-ai-prp PRPs/generated_prp.md
```

如果你未使用 Claude Code，可以让你的 AI 编程助手直接用 .claude/commands 目录下的 generate-pydantic-ai-prp 和 execute-pydantic-ai-prp 命令作为提示词。

## 📖 模板简介

本模板为你构建复杂的 Pydantic AI 智能体提供一切所需，采用成熟的上下文工程工作流，集成：

- **Pydantic AI 最佳实践**：类型安全的智能体，工具集成，结构化输出，依赖注入
- **上下文工程工作流**：成熟的 PRP（产品需求提示）方法论
- **工作示例**：完整的智能体实现，可学习和扩展

## 🎯 PRP 框架工作流

本模板采用三步上下文工程工作流构建智能体：

### 1. **定义需求**（`PRPs/INITIAL.md`）
首先明确你的智能体要做什么：
```markdown
# 客服智能体 - 初始需求

## 概述
构建一个能处理咨询、访问客户数据并适当升级问题的智能客服智能体。

## 核心需求
- 多轮对话，具备上下文和记忆
- 客户身份验证与账户访问
- 账户余额与交易查询
- 支付处理与退款操作
...
```

### 2. **生成实现方案**
```bash
/generate-pydantic-ai-prp PRPs/INITIAL.md
```
这将生成一份全面的“产品需求提示”文档，内容包括：
- Pydantic AI 技术调研与最佳实践
- 智能体架构设计（工具与依赖）
- 实现路线图与验证循环
- 安全模式与生产注意事项

### 3. **执行实现**
```bash
/execute-pydantic-ai-prp PRPs/your_agent.md
```
根据 PRP 实现完整智能体，包括：
- 智能体创建与模型提供方配置
- 工具集成、错误处理与校验
- 结构化输出模型与 Pydantic 校验
- TestModel 和 FunctionModel 全面测试

## 📂 模板结构

```
pydantic-ai/
├── CLAUDE.md                           # Pydantic AI 全局开发规范
├── copy_template.py                    # 模板部署脚本
├── .claude/commands/
│   ├── generate-pydantic-ai-prp.md     # 智能体 PRP 生成
│   └── execute-pydantic-ai-prp.md      # 智能体 PRP 执行
├── PRPs/
│   ├── templates/
│   │   └── prp_pydantic_ai_base.md     # 智能体 PRP 基础模板
│   └── INITIAL.md                      # 智能体需求示例
├── examples/
│   ├── basic_chat_agent/               # 简单对话智能体
│   │   ├── agent.py                    # 具备记忆和上下文的智能体
│   │   └── README.md                   # 使用说明
│   ├── tool_enabled_agent/             # 工具增强型智能体
│   │   ├── agent.py                    # 集成网页搜索和计算器
│   │   └── requirements.txt            # 依赖
│   └── testing_examples/               # 全面测试模式
│       ├── test_agent_patterns.py      # TestModel、FunctionModel 示例
│       └── pytest.ini                  # 测试配置
└── README.md                           # 本文件
```

## 🤖 智能体示例

### 1. 主智能体参考（`examples/main_agent_reference/`）
**权威参考实现**，展示标准 Pydantic AI 模式：
- 基于环境变量的配置（`settings.py`、`providers.py`）
- 邮件与调研智能体关注点分离
- 按文件类型分离 prompts、tools、agents、Pydantic models
- 工具集成外部 API（Gmail、Brave Search）

**关键文件：**
- `settings.py`：基于 pydantic-settings 的环境配置
- `providers.py`：模型提供方抽象，`get_llm_model()`
- `research_agent.py`：多工具集成，网页搜索+邮件
- `email_agent.py`：Gmail 草稿专用智能体

### 2. 基础对话智能体（`examples/basic_chat_agent/`）
简单对话智能体，演示核心模式：
- **基于环境变量的模型配置**（同 main_agent_reference）
- **默认字符串输出**（无 result_type）
- 系统提示词（静态/动态）
- 依赖注入实现对话记忆

**主要特性：**
- 简单字符串响应（非结构化输出）
- 基于 settings 的配置模式
- 对话上下文追踪
- 简洁实现

### 3. 工具增强型智能体（`examples/tool_enabled_agent/`）
具备工具集成能力的智能体：
- **基于环境变量的配置**（同 main_agent_reference）
- **默认字符串输出**（无多余结构）
- 集成网页搜索和计算工具
- 错误处理与重试机制

**主要特性：**
- `@agent.tool` 装饰器模式
- RunContext 依赖注入
- 工具错误处理与恢复
- 工具返回简单字符串

### 4. 结构化输出智能体（`examples/structured_output_agent/`）
**新示例**：展示何时用 `result_type` 进行数据校验：
- **基于环境变量的配置**（同 main_agent_reference）
- **结构化输出+Pydantic 校验**（仅在确实需要时）
- 数据分析与统计工具
- 专业报告生成

**主要特性：**
- 正确使用 `result_type`
- Pydantic 校验业务报告
- 数据分析工具与数值统计
- 何时用结构化/字符串输出有清晰说明

### 5. 测试示例（`examples/testing_examples/`）
Pydantic AI 智能体全面测试模式：
- TestModel 快速开发验证
- FunctionModel 自定义行为测试
- Agent.override() 测试隔离
- Pytest fixtures 与异步测试

**主要特性：**
- 单元测试无需 API 成本
- 依赖注入 mock
- 工具校验与错误场景测试
- 集成测试模式

## 📚 其他资源

- **Pydantic AI 官方文档**：https://ai.pydantic.dev/
- **上下文工程方法论**：见主仓库 README

## 🆘 支持与贡献

- **问题反馈**：模板或示例有问题请提交 issue
- **改进建议**：欢迎贡献更多示例或模式
- **疑问咨询**：关于 Pydantic AI 集成或上下文工程可提问

本模板属于更大的上下文工程框架。更多上下文工程模板和方法论请见主仓库。

---

**准备好构建生产级 AI 智能体了吗？** 直接运行 `python copy_template.py my-agent-project` 并按 PRP 工作流推进吧！🚀