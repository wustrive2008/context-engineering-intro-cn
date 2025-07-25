# 上下文工程模板

一个全面的模板，用于开始使用上下文工程 - 为AI编程助手设计上下文的学科，使它们拥有完成任务所需的全部信息。

> **上下文工程比提示词工程好10倍，比氛围编程好100倍。**

## 🚀 快速开始

```bash
# 1. 克隆此模板
git clone https://github.com/wustrive2008/context-engineering-intro-cn.git
cd context-engineering-intro-cn

# 2. 设置项目规则（可选 - 已提供模板）
# 编辑 CLAUDE.md 添加您的项目特定指南

# 3. 添加示例（强烈推荐）
# 将相关代码示例放在 examples/ 文件夹中

# 4. 创建初始功能需求
# 编辑 INITIAL.md 填写您的功能需求

# 5. 生成全面的PRP（产品需求提示词）
# 在 Claude Code 中运行：
/generate-prp INITIAL.md

# 6. 执行PRP实现您的功能
# 在 Claude Code 中运行：
/execute-prp PRPs/your-feature-name.md
```

## 📚 目录

- [什么是上下文工程？](#什么是上下文工程)
- [模板结构](#模板结构)
- [分步指南](#分步指南)
- [编写有效的INITIAL.md文件](#编写有效的INITIALmd文件)
- [PRP工作流程](#PRP工作流程)
- [有效使用示例](#有效使用示例)
- [最佳实践](#最佳实践)

## 什么是上下文工程？

上下文工程代表了从传统提示词工程的范式转变：

### 提示词工程 vs 上下文工程

**提示词工程：**
- 专注于巧妙的措辞和特定的表达
- 仅限于如何表述任务
- 就像给别人一张便利贴

**上下文工程：**
- 提供全面上下文的完整系统
- 包括文档、示例、规则、模式和验证
- 就像编写包含所有细节的完整剧本

### 为什么上下文工程很重要

1. **减少AI失败**：大多数Agent失败不是模型失败 - 而是上下文失败
2. **确保一致性**：AI遵循您的项目模式和约定
3. **启用复杂功能**：通过适当的上下文，AI可以处理多步骤实现
4. **自我纠正**：验证循环允许AI修复自己的错误

## 模板结构

```
context-engineering-intro/
├── .claude/
│   ├── commands/
│   │   ├── generate-prp.md    # 生成全面的PRP
│   │   └── execute-prp.md     # 执行PRP实现功能
│   └── settings.local.json    # Claude Code 权限
├── PRPs/
│   ├── templates/
│   │   └── prp_base.md       # PRP基础模板
│   └── EXAMPLE_multi_agent_prp.md  # 完整PRP示例
├── examples/                  # 您的代码示例（关键！）
├── CLAUDE.md                 # AI助手的全局规则
├── INITIAL.md               # 功能需求模板
├── INITIAL_EXAMPLE.md       # 功能需求示例
└── README.md                # 此文件
```

此模板不专注于RAG和上下文工程工具，因为我很快会有更多内容。;)

## 分步指南

### 1. 设置全局规则（CLAUDE.md）

`CLAUDE.md` 文件包含 AI 助手在每次对话中都要遵循的项目级规则。模板包括：

- **项目感知**：读取规划文档，检查任务
- **代码结构**：文件大小限制，模块组织
- **测试要求**：单元测试模式，覆盖率期望
- **风格约定**：语言偏好，格式化规则
- **文档标准**：文档字符串格式，注释规范

**你可以直接使用提供的模板，也可以根据项目自定义。**

### 2. 创建初始功能需求

编辑 `INITIAL.md`，描述你想要构建的内容：

```markdown
## FEATURE:
[描述你要构建的内容——请具体说明功能和需求]

## EXAMPLES:
[列出 examples/ 文件夹中的示例文件，并说明如何使用]

## DOCUMENTATION:
[包含相关文档、API 或 MCP 服务器资源的链接]

## OTHER CONSIDERATIONS:
[提及注意事项、特殊需求或 AI 助手常见遗漏]
```

**完整示例见 `INITIAL_EXAMPLE.md`。**

### 3. 生成 PRP

PRP（产品需求提示词）是包含以下内容的全面实现蓝图：

- 完整的上下文和文档
- 带有验证的实现步骤
- 错误处理模式
- 测试要求

它类似于 PRD（产品需求文档），但更专门用于指导 AI 编程助手。

在 Claude Code 中运行：
```bash
/generate-prp INITIAL.md
```

**注意：** 斜杠命令是在 `.claude/commands/` 中自定义的。你可以查看它们的实现：
- `.claude/commands/generate-prp.md` - 查看如何调研并生成 PRP
- `.claude/commands/execute-prp.md` - 查看如何根据 PRP 实现功能

这些命令中的 `$ARGUMENTS` 变量会接收你在命令名后输入的内容（如 `INITIAL.md` 或 `PRPs/your-feature.md`）。

此命令将会：
1. 读取你的功能需求
2. 调研代码库中的模式
3. 搜索相关文档
4. 在 `PRPs/your-feature-name.md` 中生成全面的 PRP

### 4. 执行 PRP

生成后，执行 PRP 以实现你的功能：

```bash
/execute-prp PRPs/your-feature-name.md
```

AI 编程助手将会：
1. 读取 PRP 的全部上下文
2. 创建详细的实现计划
3. 逐步执行并验证每一步
4. 运行测试并修复问题
5. 确保所有成功标准都达成

## 编写有效的INITIAL.md文件

### 关键部分说明

**FEATURE**: 请具体和全面
- ❌ "构建一个网络爬虫"
- ✅ "构建一个使用BeautifulSoup的异步网络爬虫，提取电子商务网站的产品数据，处理速率限制，并将结果存储在PostgreSQL中"

**EXAMPLES**: 利用 examples/ 文件夹
- 将相关代码模式放在 `examples/` 中
- 参考特定文件和模式
- 解释应模仿哪些方面

**DOCUMENTATION**: 包含所有相关资源
- API 文档 URL
- 库指南
- MCP 服务器文档
- 数据库模式

**OTHER CONSIDERATIONS**: 捕捉重要细节
- 认证要求
- 速率限制或配额
- 常见陷阱
- 性能要求

## PRP工作流程

### /generate-prp 的工作原理

该命令遵循以下过程：

1. **调研阶段**
   - 分析您的代码库中的模式
   - 搜索类似的实现
   - 确定要遵循的约定

2. **文档收集**
   - 获取相关的API文档
   - 包括库文档
   - 添加注意事项和特殊情况

3. **蓝图创建**
   - 创建逐步实施计划
   - 包括验证环节
   - 添加测试要求

4. **质量检查**
   - 评分信心级别（1-10）
   - 确保包含所有上下文

### /execute-prp 的工作原理

1. **加载上下文**：读取整个 PRP
2. **计划**：使用 TodoWrite 创建详细任务列表
3. **执行**：实现每个组件
4. **验证**：运行测试和代码检查
5. **迭代**：修复发现的任何问题
6. **完成**：确保满足所有要求

有关生成内容的完整示例，请参见 `PRPs/EXAMPLE_multi_agent_prp.md`。

## 有效使用示例

`examples/` 文件夹对成功至关重要。当 AI 编程助手可以看到要遵循的模式时，它们的表现会更好。

### 示例中应包含的内容

1. **代码结构模式**
   - 您如何组织模块
   - 导入约定
   - 类/函数模式

2. **测试模式**
   - 测试文件结构
   - 模拟方法
   - 断言风格

3. **集成模式**
   - API 客户端实现
   - 数据库连接
   - 认证流程

4. **CLI 模式**
   - 参数解析
   - 输出格式
   - 错误处理

### 示例结构

```
examples/
├── README.md           # 解释每个示例演示的内容
├── cli.py             # CLI 实现模式
├── agent/             # 代理架构模式
│   ├── agent.py      # 代理创建模式
│   ├── tools.py      # 工具实现模式
│   └── providers.py  # 多提供者模式
└── tests/            # 测试模式
    ├── test_agent.py # 单元测试模式
    └── conftest.py   # Pytest 配置
```

## 最佳实践

### 1. 在 INITIAL.md 中明确
- 不要假设 AI 知道您的偏好
- 包括具体的要求和限制
- 自由地引用示例

### 2. 提供全面的示例
- 示例越多，实现效果越好
- 同时展示该做什么和不该做什么
- 包括错误处理模式

### 3. 使用验证环节
- PRP 包括必须通过的测试命令
- AI 将迭代直到所有验证成功
- 这确保了第一次就能得到可用的代码

### 4. 利用文档
- 包括官方 API 文档
- 添加 MCP 服务器资源
- 参考特定文档部分

### 5. 自定义 CLAUDE.md
- 添加您的约定
- 包括项目特定规则
- 定义编码标准

## 资源

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Context Engineering Best Practices](https://www.philschmid.de/context-engineering)