<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 为claude如何做贡献

感谢您有兴趣为此项目做出贡献！本指南将帮助您了解如何有效地做出贡献。

## 关于这个项目

Claude How To 是 Claude Code 的直观、示例驱动指南。我们提供：
- **Mermaid图**解释功能如何工作
- **生产就绪模板**您可以立即使用
- **真实世界的例子**以及背景和最佳实践
- **渐进式学习路径**从初学者到高级

## 贡献类型

### 1. 新示例或模板
添加现有功能的示例（斜杠命令、skills、hooks等）：
- 复制粘贴准备好的代码
- 清晰解释其工作原理
- 使用案例和好处
- 故障排除提示

### 2. 文档改进
- 澄清令人困惑的部分
- 修复拼写错误和语法
- 添加缺失的信息
- 改进代码示例

### 3. 功能指南
为新的 Claude Code 功能创建指南：
- 分步教程
- 架构图
- 常见模式和反模式
- 真实的工作流程

### 4. 错误报告
报告您遇到的问题：
- 描述你的期望
- 描述实际发生的事情
- 包括重现步骤
- 添加相关的claude代码版本和操作系统

### 5.反馈与建议
帮助改进指南：
- 建议更好的解释
- 指出覆盖范围的差距
- 推荐新的部分或重组

## 开始使用

### 1. 分叉和克隆
```bash
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto
```
### 2. 创建分支
使用描述性分支名称：
```bash
git checkout -b add/feature-name
git checkout -b fix/issue-description
git checkout -b docs/improvement-area
```
### 3. 设置您的环境
```bash
# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install pre-commit hooks (optional but recommended)
pip install pre-commit
pre-commit install

# Run pre-commit checks manually
pre-commit run --all-files
```
## 目录结构
```
├── 01-slash-commands/      # User-invoked shortcuts
├── 02-memory/              # Persistent context examples
├── 03-skills/              # Reusable capabilities
├── 04-subagents/           # Specialized AI assistants
├── 05-mcp/                 # Model Context Protocol examples
├── 06-hooks/               # Event-driven automation
├── 07-plugins/             # Bundled features
├── 08-checkpoints/         # Session snapshots
├── 09-advanced-features/   # Planning, thinking, backgrounds
├── 10-cli/                 # CLI reference
├── scripts/                # Build and utility scripts
└── README.md               # Main guide
```
## 如何贡献示例

### 添加斜线命令
1、在`01-slash-commands/`中创建一个`.md`文件
2. 包括：
   - 清楚地描述它的作用
   - 使用案例
   - 安装说明
   - 使用示例
   - 定制技巧
3.更新`01-slash-commands/README.md`

### 添加skills
1. 在`03-skills/`中创建目录
2. 包括：
   - `SKILL.md` - 主要文档
   - `scripts/` - 帮助脚本（如果需要）
   - `templates/` - 提示模板
   - 自述文件中的示例用法
3.更新`03-skills/README.md`

### 添加Subagents
1.在`04-subagents/`中创建一个`.md`文件
2. 包括：
   - subagents的目的和能力
   - 系统提示结构
   - 示例用例
   - 集成示例
3.更新`04-subagents/README.md`

### 添加 MCP 配置
1.在`05-mcp/`中创建一个`.json`文件
2. 包括：
   - 配置说明
   - 所需的环境变量
   - 设置说明
   - 使用示例
3.更新`05-mcp/README.md`

### 添加一个Hook
1.在`06-hooks/`中创建一个`.sh`文件
2. 包括：
   - Shebang 和描述
   - 清晰的注释解释逻辑
   - 错误处理
   - 安全考虑
3.更新`06-hooks/README.md`

## 写作指南

### Markdown 风格
- 使用清晰的标题（H2 表示章节，H3 表示小节）
- 保持段落简短且重点突出
- 对列表使用项目符号点
- 包含带有语言规范的代码块
- 在部分之间添加空行

### 代码示例
- 准备好示例复制粘贴
- 评论不明显的逻辑
- 包括简单版和高级版
- 展示真实世界的用例
- 突出潜在问题

### 文档
- 解释“为什么”而不仅仅是“什么”
- 包括先决条件
- 添加故障排除部分
- 相关主题的链接
- 保持初学者友好

### JSON/YAML
- 使用适当的缩进（一致的 2 或 4 个空格）
- 添加解释配置的注释
- 包括验证示例

### 图表
- 尽可能使用Mermaid
- 保持图表简单易读
- 包括图表下方的描述
- 相关部分的链接

## 提交指南

遵循常规提交格式：
```
type(scope): description

[optional body]
```
类型：
- `feat`：新功能或示例
- `fix`：错误修复或更正
- `docs`：文档更改
- `refactor`：代码重组
- `style`：格式更改
- `test`：测试添加或更改
- `chore`：构建、依赖项等。

示例：
```
feat(slash-commands): Add API documentation generator
docs(memory): Improve personal preferences example
fix(README): Correct table of contents link
docs(skills): Add comprehensive code review skill
```
## 提交之前

### 清单
- [ ] 代码遵循项目风格和约定
- [ ] 新示例包括清晰的文档
- [ ] README 文件已更新（本地和根）
- [ ] 没有敏感信息（API 密钥、凭证）
- [ ] 示例已测试且有效
- [ ] 链接已验证且正确
- [ ] 文件具有适当的权限（脚本可执行）
- [ ] 提交消息清晰且具有描述性

### 本地测试
```bash
# Check file formatting
pre-commit run --all-files

# Verify links work (if applicable)
# Test examples manually with Claude Code

# Review your changes
git diff

# Test the EPUB generation (if docs changed)
uv run scripts/build_epub.py
```
## 拉取请求流程

1. **创建具有清晰描述的 PR**：
   - 这增加/修复了什么？
   - 为什么需要它？
   - 相关问题（如果有）

2. **包括相关详细信息**：
   - 新功能？包括用例
   - 文档？解释改进之处
   - 例子？显示之前/之后

3. **问题链接**：
   - 使用`Closes #123`自动关闭相关问题

4. **耐心等待评论**：
   - 维护者可能会提出改进建议
   - 根据反馈进行迭代
   - 最终决定权在于维护者

## 代码审查流程

评审员将检查：
- **准确性**：它是否按描述工作？
- **质量**：是否可以投入生产？
- **一致性**：它遵循项目模式吗？
- **文档**：是否清晰且完整？
- **安全**：是否存在任何漏洞？

## 报告问题

### 错误报告
包括：
- claude代码版本
- 操作系统
- 重现步骤
- 预期行为
- 实际行为
- 屏幕截图（如果适用）

### 功能请求
包括：
- 正在解决的用例或问题
- 提议的解决方案
- 您考虑过的替代方案
- 额外的背景信息

### 文档问题
包括：
- 有什么令人困惑或遗漏的地方
- 改进建议
- 示例或参考

## 项目政策

### 敏感信息
- 切勿提交 API 密钥、Token或凭据
- 在示例中使用占位符值
- 配置文件包含 `.env.example`
- 记录所需的环境变量

### 代码质量
- 保持示例的重点和可读性
- 避免过度设计解决方案
- 包括对不明显逻辑的注释
- 提交前彻底测试

### 知识产权
- 原创内容归作者所有
- 项目使用教育许可证
- 尊重现有版权
- 在需要时提供归属

## 获取帮助

- **问题**：在 GitHub 问题中打开讨论
- **一般帮助**：检查现有文档
- **开发帮助**：查看类似示例
- **代码审查**：PR 中的标签维护者

## 认可

贡献者在以下领域获得认可：
- README.md 贡献者部分
- GitHub 贡献者页面
- 提交历史记录

## 安全

贡献示例和文档时，请遵循安全编码实践：

- **切勿对机密或 API 密钥进行硬编码** - 使用环境变量
- **警告安全影响** - 突出显示潜在风险
- **使用安全默认设置** - 默认启用安全功能
- **验证输入** - 显示正确的输入验证和清理
- **包括安全说明** - 记录安全注意事项

对于安全问题，请参阅 [SECURITY.md](SECURITY.md) 了解我们的漏洞报告流程。

＃＃ 行为守则

我们致力于提供一个热情和包容的社区。请阅读 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 了解我们完整的社区标准。

简而言之：
- 尊重和包容
- 优雅地欢迎反馈
- 帮助他人学习和成长
- 避免骚扰或歧视
- 向维护人员报告问题

所有贡献者都应遵守此准则并以友善和尊重的态度对待彼此。

## 许可证

通过为本项目做出贡献，您同意您的贡献将根据 MIT 许可证获得许可。有关详细信息，请参阅 [LICENSE](LICENSE) 文件。

＃＃ 问题？

- 检查 [README](README.md)
- 回顾 [LEARNING-ROADMAP.md](LEARNING-ROADMAP.md)
- 查看现有示例
- 打开一个问题进行讨论

感谢您的贡献！ 🙏
