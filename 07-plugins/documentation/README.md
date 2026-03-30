<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 文档Plugins

为您的项目提供全面的文档生成和维护。

## 特点

✅ API 文档生成
✅ 自述文件创建和更新
✅ 文档同步
✅ 代码注释改进
✅ 示例生成

## 安装
```bash
/plugin install documentation
```
## 包含什么

### 斜线命令
- `/generate-api-docs` - 生成API文档
- `/generate-readme` - 创建或更新自述文件
- `/sync-docs` - 将文档与代码更改同步
- `/validate-docs` - 验证文档

### Subagents
- `api-documenter` - API 文档专家
- `code-commentator` - 代码注释改进
- `example-generator` - 代码示例创建

### 模板
- `api-endpoint.md` - API 端点文档模板
- `function-docs.md` - 功能文档模板
- `adr-template.md` - 架构决策记录模板

### MCP 服务器
- GitHub 集成用于文档同步

## 用法

### 生成API文档
```
/generate-api-docs
```
### 创建自述文件
```
/generate-readme
```
### 同步文档
```
/sync-docs
```
### 验证文档
```
/validate-docs
```
## 要求

- claude代码 1.0+
- GitHub 访问（可选）

## 工作流程示例
```
User: /generate-api-docs

Claude:
1. Scans all API endpoints in /src/api/
2. Delegates to api-documenter subagent
3. Extracts function signatures and JSDoc
4. Organizes by module/endpoint
5. Uses api-endpoint.md template
6. Generates comprehensive markdown docs
7. Includes curl, JavaScript, and Python examples

Result:
✅ API documentation generated
📄 Files created:
   - docs/api/users.md
   - docs/api/auth.md
   - docs/api/products.md
📊 Coverage: 23/23 endpoints documented
```
## 模板使用

### API 端点模板
用于通过完整示例记录 REST API 端点。

### 函数文档模板
用于记录各个函数/方法。

### ADR 模板
用于记录架构决策。

## 配置

设置 GitHub Token以进行文档同步：
```bash
export GITHUB_TOKEN="your_github_token"
```
## 最佳实践

- 让文档靠近代码
- 通过代码更改更新文档
- 包括实际例子
- 定期验证
- 使用模板以保持一致性
