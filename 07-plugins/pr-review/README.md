<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# PR 审查Plugins

完整的公关审查工作流程，包括安全、测试和文档检查。

## 特点

✅ 安全分析
✅ 测试覆盖率检查
✅ 文件验证
✅ 代码质量评估
✅ 绩效影响分析

## 安装
```bash
/plugin install pr-review
```
## 包含什么

### 斜线命令
- `/review-pr` - 全面公关审查
- `/check-security` - 以安全为重点的审查
- `/check-tests` - 测试覆盖率分析

### Subagents
- `security-reviewer` - 安全漏洞检测
- `test-checker` - 测试覆盖率分析
- `performance-analyzer` - 绩效影响评估

### MCP 服务器
- PR 数据的 GitHub 集成

### hooks
- `pre-review.js` - 预审验证

## 用法

### 基本公关审查
```
/review-pr
```
### 仅安全检查
```
/check-security
```
### 测试覆盖率检查
```
/check-tests
```
## 要求

- claude代码 1.0+
- GitHub 访问
- Git 存储库

## 配置

设置您的 GitHub Token：
```bash
export GITHUB_TOKEN="your_github_token"
```
## 工作流程示例
```
User: /review-pr

Claude:
1. Runs pre-review hook (validates git repo)
2. Fetches PR data via GitHub MCP
3. Delegates security review to security-reviewer subagent
4. Delegates testing to test-checker subagent
5. Delegates performance to performance-analyzer subagent
6. Synthesizes all findings
7. Provides comprehensive review report

Result:
✅ Security: No critical issues found
⚠️  Testing: Coverage is 65%, recommend 80%+
✅ Performance: No significant impact
📝 Recommendations: Add tests for edge cases
```
