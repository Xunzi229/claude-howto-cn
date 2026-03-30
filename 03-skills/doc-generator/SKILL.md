---
名称： api 文档生成器
描述：从源代码生成全面、准确的 API 文档。在创建或更新 API 文档、生成 OpenAPI 规范或用户提及 API 文档、端点或文档时使用。
---

# API 文档生成skills

## 生成

- OpenAPI/Swagger 规范
- API端点文档
- SDK使用示例
- 集成指南
- 错误代码参考
- 身份验证指南

## 文档结构

### 对于每个端点
```markdown
## GET /api/v1/users/:id

### Description
Brief explanation of what this endpoint does

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| id | string | Yes | User ID |

### Response

**200 Success**
```json
{
“id”：“usr_123”
  “姓名”：“约翰·多伊”，
“电子邮件”：“john@example.com”，
“创建时间”：“2025-01-15T10:30:00Z”
}
```

**404 Not Found**
```json
{
“错误”：“USER_NOT_FOUND”，
"message": "用户不存在"
}
```

### Examples

**cURL**
```bash
卷曲-X GET“https://api.example.com/api/v1/users/usr_123" \
-H“授权：持有者YOUR_TOKEN”
```

**JavaScript**
```javascript
const user = 等待 fetch('/api/v1/users/usr_123', {
headers: { '授权': '不记名Token' }
}).then(r => r.json());
```

**Python**
```python
响应 = requests.get(
'https://api.example.com/api/v1/users/usr_123',
headers={'授权': '不记名Token'}
）
用户=response.json()
```
```
