# 项目配置

## 项目概述
- **名称**：电子商务平台
- **技术堆栈**：Node.js、PostgreSQL、React 18、Docker
- **团队规模**：5 名开发人员
- **截止日期**：2025 年第 4 季度

＃＃ 建筑学
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## 开发标准

### 代码风格
- 使用 Prettier 进行格式化
- 将 ESLint 与 Airbnb 配置结合使用
- 最大行长度：100 个字符
- 使用 2 个空格缩进

### 命名约定
- **文件**：kebab-case (user-controller.js)
- **类**：PascalCase (UserService)
- **函数/变量**：camelCase (getUserById)
- **常量**：UPPER_SNAKE_CASE (API_BASE_URL)
- **数据库表**：snake_case (user_accounts)

### Git 工作流程
- 分支名称：`feature/description` 或 `fix/description`
- 提交消息：遵循常规提交
- 合并前需要 PR
- 所有 CI/CD 检查必须通过
- 至少需要 1 次批准

### 测试要求
- 至少 80% 的代码覆盖率
- 所有关键路径都必须经过测试
- 使用 Jest 进行单元测试
- 使用Cypress进行E2E测试
- 测试文件名：`*.test.ts` 或 `*.spec.ts`

### API 标准
- 仅限 RESTful 端点
- JSON 请求/响应
- 正确使用HTTP状态码
- 版本 API 端点：`/api/v1/`
- 用示例记录所有端点

＃＃＃ 数据库
- 使用迁移进行架构更改
- 切勿对凭据进行硬编码
- 使用连接池
- 在开发中启用查询日志记录
- 需要定期备份

### 部署
- 基于Docker的部署
- Kubernetes编排
- 蓝绿部署策略
- 失败时自动回滚
- 数据库迁移在部署之前运行

## 常用命令

|命令 |目的|
|---------|---------|
| `npm run dev` |启动开发服务器 |
| `npm test` |运行测试套件 |
| `npm run lint` |检查代码风格|
| `npm run build` |为生产而构建 |
| `npm run migrate` |运行数据库迁移 |

## 团队联系方式
- 技术主管：Sarah Chen (@sarah.chen)
- 产品经理：Mike Johnson (@mike.j)
- DevOps：Alex Kim (@alex.k)

## 已知问题和解决方法
- 高峰时段 PostgreSQL 连接池限制为 20 个
- 解决方法：实施查询队列
- Safari 14 与异步生成器的兼容性问题
- 解决方法：使用 Babel 转译器

## 相关项目
- 分析仪表板：`/projects/analytics`
- 移动应用程序：`/projects/mobile`
- 管理面板：`/projects/admin`
