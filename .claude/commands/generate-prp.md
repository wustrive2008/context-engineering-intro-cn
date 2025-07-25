# 创建PRP

## 功能文件：$ARGUMENTS

根据功能文件生成完整的PRP，涵盖全面的研究。确保将上下文传递给AI代理，使其能够进行自我验证和迭代改进。阅读功能文件以理解需要创建的内容，示例如何帮助实现，以及任何其他注意事项。

AI代理仅获得你附加到PRP和训练数据中的上下文。假设AI代理可以访问代码库，并具有与你相同的知识截止日期，因此重要的是将你的研究成果包含或引用到PRP中。代理具有网络搜索能力，因此请传递文档和示例的URL。

## 研究过程

1. **代码库分析**
   - 在代码库中搜索类似的功能/模式
   - 确定要在PRP中引用的文件
   - 注意现有的约定以遵循
   - 检查测试模式以确定验证方法

2. **外部研究**
   - 在网上搜索类似的功能/模式
   - 库文档（包括具体URL）
   - 实现示例（GitHub/StackOverflow/博客）
   - 最佳实践和常见问题

3. **用户澄清**（如果需要）
   - 特定模式要镜像以及在哪里找到它们？
   - 集成要求以及在哪里找到它们？

## PRP生成

使用PRPs/templates/prp_base.md作为模板：

### 需要包含并传递给AI代理的关键上下文
- **文档**：带有具体部分的URL
- **代码示例**：来自代码库的实际代码片段
- **注意事项**：库的陷阱，版本问题
- **模式**：要遵循的现有方法

### 实现蓝图
- 从显示方法的伪代码开始
- 引用真实文件中的模式
- 包括错误处理策略
- 列出按顺序完成以实现PRP的任务

### 验证门（必须可执行）例如用于Python
```bash
# 语法/风格
ruff check --fix && mypy .

# 单元测试
uv run pytest tests/ -v

```

*** 在你完成研究和探索代码库之后，在开始编写PRP之前 ***

*** 深思熟虑PRP并规划你的方法，然后开始编写PRP ***

## 输出
保存为：`PRPs/{feature-name}.md`

## 质量检查清单
- [ ] 包含所有必要的上下文
- [ ] 验证门对AI可执行
- [ ] 引用现有模式
- [ ] 清晰的实现路径
- [ ] 文档记录错误处理

在通过全面上下文实现一次通过实施之前，对PRP进行1-10分评分（成功信心水平）

记住：目标是通过全面的上下文实现一次通过实施。
