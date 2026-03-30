# 安全策略

## 概述

Claude How To 项目的安全对我们很重要。本文档概述了我们的安全实践并描述了如何负责任地报告安全漏洞。

## 支持的版本

我们为以下版本提供安全更新：

|版本 |状态 |支持直到 |
|--------|--------|---------------|
|最新（主要）| ✅ 活跃 |当前 + 6 个月 |
| 1.x 版本 | ✅ 活跃 |直到下一个主要版本 |

**注意**：作为一个教育指南项目，我们专注于维护当前的最佳实践和文档安全性，而不是传统的版本支持。更新直接应用于主分支。

## 安全实践

### 代码安全

1. **依赖管理**
   - 所有 Python 依赖项都固定在 `requirements.txt` 中
   - 通过dependabot和手动审核定期更新
   - 每次提交时都使用 Bandit 进行安全扫描
   - 用于安全检查的预提交hooks

2. **代码质量**
   - 使用 Ruff 进行 Linting 捕获常见问题
   - 使用 mypy 进行类型检查可防止与类型相关的漏洞
   - 预提交hooks执行标准
   - 合并前审查所有更改

3. **访问控制**
   - `main` 分支上的分支保护
   - 合并前需要进行审查
   - 合并前必须通过状态检查
   - 对存储库的写访问权限有限

### 文档安全

1. **例子中没有秘密**
   - 示例中的所有 API 密钥均为占位符
   - 凭证绝不会被硬编码
   - `.env.example` 文件显示所需的变量
   - 秘密管理的明确警告

2. **安全最佳实践**
   - 示例演示安全模式
   - 文档中突出显示的安全警告
   - 官方安全指南的链接
   - 相关章节中讨论的凭证处理

3. **内容审核**
   - 审查所有文档的安全问题
   - 贡献指南时的安全考虑
   - 外部链接和参考的验证

### 依赖安全

1. **扫描**
   - Bandit 扫描所有 Python 代码是否存在漏洞
   - 通过 GitHub 安全警报进行依赖漏洞检查
   - 定期人工安全审核

2. **更新**
   - 及时应用安全补丁
   - 主要版本经过仔细评估
   - 变更日志包括与安全相关的更新

3. **透明度**
   - 提交中记录的安全更新
   - 负责任地处理漏洞披露
   - 适当时的公共安全咨询

## 报告漏洞

### 我们关心的安全问题

我们感谢以下方面的报告：
- 脚本或示例中的**代码漏洞**
- Python 包中的 **依赖漏洞**
- 任何代码示例中的 **加密问题**
- 文档中的**身份验证/授权缺陷**
- 配置示例中的**数据暴露风险**
- **注入漏洞**（SQL、命令等）
- **SSRF/XXE/路径遍历**问题

### 安全问题超出范围

这些不属于该项目的范围：
- Claude 代码本身的漏洞（向 Anthropic 报告）
- 外部服务或库的问题（向上游报告）
- 社会工程或用户教育（不适用于本指南）
- 没有概念证明的理论漏洞
- 通过官方渠道报告的依赖项中的漏洞

## 如何举报

### 私人报告（首选）

**对于敏感安全问题，请使用GitHub的私有漏洞报告：**

1. 访问：https://github.com/luongnv89/claude-howto/security/advisories
2. 点击“报告漏洞”
3.填写漏洞详情
4. 包括：
   - 清晰的漏洞描述
   - 受影响的组件（文件、部分、示例）
   - 潜在影响
   - 重现步骤（如果适用）
   - 建议的修复（如果有的话）

**接下来会发生什么：**
- 我们将在 48 小时内确认收货
- 我们将调查并评估严重性
- 我们将与您合作开发修复方案
- 我们将协调披露时间表
- 我们将在安全建议中注明您的姓名（除非您希望匿名）

### 公开报告

对于非敏感问题或已经公开的问题：

1. **打开带有标签 `security` 的 GitHub 问题**
2. 包括：
   - 标题：`[SECURITY]` 后面是简短说明
   - 详细描述
   - 受影响的文件或部分
   - 潜在影响
   - 建议修复

## 漏洞响应流程

### 评估（24 小时）

1. 我们确认收到报告
2. 我们使用 [CVSS v3.1](https://www.first.org/cvss/v3.1/specification-document) 评估严重性
3.我们确定它是否在范围内
4. 我们与您联系进行初步评估

### 开发（1-7 天）

1.我们开发一个修复方案
2. 我们审查并测试修复
3. 我们创建安全建议
4. 我们准备发行说明

### 披露（因严重程度而异）

**严重（CVSS 9.0-10.0）**
- 修复立即发布
- 发布公共咨询
- 提前24小时通知记者

**高（CVSS 7.0-8.9）**
- 修复将在 48-72 小时内发布
- 提前 5 天通知记者
- 关于发布的公共咨询

**中（CVSS 4.0-6.9）**
- 修复在下次定期更新中发布
- 关于发布的公共咨询

**低（CVSS 0.1-3.9）**
- 修复包含在下一次定期更新中
- 发布咨询

### 出版

我们发布的安全公告包括：
- 漏洞描述
- 受影响的组件
- 严重性评估（CVSS 评分）
- 修复版本
- 解决方法（如果适用）
- 鸣谢记者（经许可）

## 记者最佳实践

### 报告之前

- **验证问题**：你能一致地重现它吗？
- **搜索现有问题**：是否已经报告？
- **检查文档**：是否有安全使用指南？
- **测试修复**：您建议的修复有效吗？

### 报告时

- **具体**：提供准确的文件路径和行号
- **包括上下文**：为什么这是一个安全问题？
- **显示影响**：攻击者可以做什么？
- **提供步骤**：我们如何重现它？
- **建议修复**：您将如何修复它？

### 报告后

- **要有耐心**：我们的资源有限
- **反应灵敏**：快速回答后续问题
- **保密**：修复前不要公开披露
- **尊重协调**：遵循我们的披露时间表

## 安全标头和配置

### 存储库安全

- **分支保护**：主分支需要 2 次变更批准
- **状态检查**：所有 CI/CD 检查必须通过
- **代码所有者**：关键文件的指定审阅者
- **签名提交**：推荐给贡献者

### 开发安全
```bash
# Install pre-commit hooks
pre-commit install

# Run security scans locally
bandit -c pyproject.toml -r scripts/
mypy scripts/ --ignore-missing-imports
ruff check scripts/
```
### 依赖安全
```bash
# Check for known vulnerabilities
pip install safety
safety check

# Or use pip-audit
pip install pip-audit
pip-audit
```
## 贡献者安全指南

### 编写示例时

1. **永远不要对秘密进行硬编码**
   ```python
   # ❌ Bad
   api_key = "sk-1234567890"

   # ✅ Good
   api_key = os.getenv("API_KEY")
   ```
2. **警告安全隐患**
   ```markdown
   ⚠️ **Security Note**: Never commit `.env` files to git.
   Add to `.gitignore` immediately.
   ```
3. **使用安全默认值**
   - 默认启用身份验证
   - 在适用的情况下使用 HTTPS
   - 验证和清理输入
   - 使用参数化查询

4. **文档安全注意事项**
   - 解释为什么安全很重要
   - 显示安全与不安全模式
   - 权威来源链接
   - 突出显示警告

### 审查贡献时

1. **检查是否有泄露的秘密**
   - 扫描常见模式（api_key=、password=）
   - 检查配置文件
   - 检查环境变量

2. **验证安全编码实践**
   - 没有硬编码的凭据
   - 正确的输入验证
   - 安全认证/授权
   - 安全的文件处理

3. **测试安全影响**
   - 这会被滥用吗？
   - 最坏的情况是什么？
   - 是否存在边缘情况？

## 安全资源

### 官方标准
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [CVSS Calculator](https://www.first.org/cvss/calculator/3.1)

### Python 安全性
- [Python Security Advisories](https://www.python.org/dev/security/)
- [PyPI Security](https://pypi.org/help/#security)
- [Bandit Documentation](https://bandit.readthedocs.io/)

### 依赖管理
- [OWASP Dependency Check](https://owasp.org/www-project-dependency-check/)
- [GitHub Security Alerts](https://docs.github.com/en/code-security/dependabot/dependabot-alerts/about-dependabot-alerts)

### 一般安全
- [Anthropic Security](https://www.anthropic.com/)
- [GitHub Security Best Practices](https://docs.github.com/en/code-security)

## 安全建议存档

过去的安全建议可在 [GitHub Security Advisories](https://github.com/luongnv89/claude-howto/security/advisories) 选项卡中找到。

## 联系方式

对于与安全相关的问题或讨论安全实践：

1. **私人安全报告**：使用GitHub的私人漏洞报告
2. **一般安全问题**：使用 `[SECURITY]` 标签打开讨论
3. **安全策略反馈**：创建带有 `security` 标签的问题

## 致谢

我们感谢帮助确保该项目安全的安全研究人员和社区成员。负责任地报告漏洞的贡献者将在我们的安全公告中得到认可（除非他们喜欢匿名）。

## 政策更新

本安全政策经过审核和更新：
- 当发现新的漏洞时
- 当安全最佳实践不断发展时
- 当项目范围发生变化时
- 至少每年一次

**最后更新**：2026 年 1 月
**下一次审核**：2027 年 1 月

---

感谢您帮助确保 Claude How To 的安全！ 🔒
