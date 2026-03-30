# 代码审查查找模板

记录代码审查期间发现的每个问题时，请使用此模板。

---

## 问题：[标题]

### 严重性
- [ ] 严重（阻止部署）
- [ ] 高（应在合并前修复）
- [ ] 中等（应该很快就会修复）
- [ ] 低（很高兴拥有）

### 类别
- [ ] 安全
- [ ] 性能
- [ ] 代码质量
- [ ] 可维护性
- [ ] 测试
- [ ] 设计模式
- [ ] 文档

### 地点
**文件：** `src/components/UserCard.tsx`

**线路：** 45-52

**功能/方法：** `renderUserDetails()`

### 问题描述

**内容：** 描述问题是什么。

**为什么重要：**解释影响以及为什么需要解决这个问题。

**当前行为：** 显示有问题的代码或行为。

**预期行为：** 描述应该发生什么。

### 代码示例

#### 当前（有问题）
```typescript
// Shows the N+1 query problem
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // Query per user!
  renderUserPosts(posts);
});
```
#### 建议的修复
```typescript
// Optimized with JOIN query
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```
### 影响分析

|方面|影响 |严重性 |
|--------|--------|----------|
|性能| 20 个用户的 100 多个查询 |高|
|用户体验 |页面加载缓慢 |高|
|可扩展性|大规模中断 |关键|
|可维护性|调试困难|中等|

### 相关问题

- `AdminUserList.tsx` 第 120 行中的类似问题
- 相关公关：#456
- 相关问题：#789

### 其他资源

- [N+1 Query Problem](https://en.wikipedia.org/wiki/N%2B1_problem)
- [Database Join Documentation](https://docs.example.com/joins)
- [Performance Optimization Guide](./docs/performance.md)

### 审稿人注释

- 这是此代码库中的常见模式
- 考虑将其添加到代码风格指南中
- 可能值得创建一个辅助函数

### 作者回应（反馈）

*由代码作者填写：*

- [ ] 修复已在提交中实施：`abc123`
- [ ] 修复状态：已完成/正在进行/需要讨论
- [ ] 问题或疑虑：（描述）

---

## 查找统计数据（供审阅者使用）

在审查多项发现时，跟踪：

- **发现的问题总数：** X
- **严重：** X
- **高：** X
- **中：** X
- **低：** X

**建议：** ✅ 批准 / ⚠️ 请求更改 / 🔄 需要讨论

**总体代码质量：** 1-5 星
