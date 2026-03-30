# API 模块标准

此文件覆盖 /src/api/ 中所有内容的根 CLAUDE.md

## API 特定标准

### 请求验证
- 使用 Zod 进行模式验证
- 始终验证输入
- 返回 400 并显示验证错误
- 包括字段级错误详细信息

### 身份验证
- 所有端点都需要 JWT Token
- 授权标头中的Token
- Token在 24 小时后过期
- 实施刷新Token机制

### 响应格式

所有响应都必须遵循以下结构：
```json
{
  "success": true,
  "data": { /* actual data */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```
错误响应：
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User message",
    "details": { /* field errors */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```
### 分页
- 使用基于光标的分页（不是偏移）
- 包括 `hasMore` 布尔值
- 将最大页面大小限制为 100
- 默认页面大小：20

### 速率限制
- 经过身份验证的用户每小时 1000 个请求
- 公共端点每小时 100 个请求
- 超出时返回429
- 包括重试后标头

### 缓存
- 使用Redis进行会话缓存
- 缓存持续时间：默认5分钟
- 写操作无效
- 使用资源类型标记缓存键
