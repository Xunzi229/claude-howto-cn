# 代码气味目录

基于 Martin Fowler 的《重构》（第二版）的代码味道综合参考。代码异味是更深层次问题的症状——它们表明代码的设计可能存在问题。

> “代码气味是一种表面迹象，通常对应于系统中更深层次的问题。” — 马丁·福勒

---

## 腹胀

代码的味道代表着一些已经变得太大而无法有效处理的东西。

### 长方法

**标志：**
- 方法超过30-50行
- 需要滚动才能看到整个方法
- 多层嵌套
- 注释解释了各部分的作用

**为什么不好：**
- 很难理解
- 难以单独测试
- 改变会产生意想不到的后果
- 重复的逻辑隐藏在里面

**重构：**
- 提取方法
- 用查询替换临时值
- 引入参数对象
- 用方法对象替换方法
- 分解条件

**示例（之前）：**
```javascript
function processOrder(order) {
  // Validate order (20 lines)
  if (!order.items) throw new Error('No items');
  if (order.items.length === 0) throw new Error('Empty order');
  // ... more validation

  // Calculate totals (30 lines)
  let subtotal = 0;
  for (const item of order.items) {
    subtotal += item.price * item.quantity;
  }
  // ... tax, shipping, discounts

  // Send notifications (20 lines)
  // ... email logic
}
```
**示例（之后）：**
```javascript
function processOrder(order) {
  validateOrder(order);
  const totals = calculateOrderTotals(order);
  sendOrderNotifications(order, totals);
  return { order, totals };
}
```
---

### 大班

**标志：**
- 类有许多实例变量（>7-10）
- 类有很多方法 (>15-20)
- 类名模糊（Manager、Handler、Processor）
- 方法不使用所有实例变量

**为什么不好：**
- 违反单一责任原则
- 难以测试
- 变化会影响到不相关的功能
- 零件难以重复使用

**重构：**
- 提取类
- 提取子类
- 提取接口

**检测：**
```
Lines of code > 300
Number of methods > 15
Number of fields > 10
```
---

### 原始的痴迷

**标志：**
- 使用原语表示域概念（字符串表示电子邮件，int表示货币）
- 基元数组而不是对象
- 类型代码的字符串常量
- 神奇的数字/字符串

**为什么不好：**
- 没有类型级别的验证
- 逻辑分散在代码库中
- 容易传递错误的值
- 缺少领域概念

**重构：**
- 用对象替换原语
- 用类别替换类型代码
- 用子类替换类型代码
- 用状态/策略替换类型代码

**示例（之前）：**
```javascript
const user = {
  email: 'john@example.com',     // Just a string
  phone: '1234567890',           // Just a string
  status: 'active',              // Magic string
  balance: 10050                 // Cents as integer
};
```
**示例（之后）：**
```javascript
const user = {
  email: new Email('john@example.com'),
  phone: new PhoneNumber('1234567890'),
  status: UserStatus.ACTIVE,
  balance: Money.cents(10050)
};
```
---

### 长参数列表

**标志：**
- 具有 4 个以上参数的方法
- 总是一起出现的参数
- 布尔标志改变方法行为
- 空/未定义频繁传递

**为什么不好：**
- 很难正确调用
- 参数顺序混乱
- 表示方法做得太多
- 难以添加新参数

**重构：**
- 引入参数对象
- 保留整个对象
- 用方法调用替换参数
- 删除标志参数

**示例（之前）：**
```javascript
function createUser(firstName, lastName, email, phone,
                    street, city, state, zip,
                    isAdmin, isActive, createdBy) {
  // ...
}
```
**示例（之后）：**
```javascript
function createUser(personalInfo, address, options) {
  // personalInfo: { firstName, lastName, email, phone }
  // address: { street, city, state, zip }
  // options: { isAdmin, isActive, createdBy }
}
```
---

### 数据块

**标志：**
- 相同的 3 个以上字段重复出现在一起
- 始终一起传播的参数
- 具有属于一起的字段子集的类

**为什么不好：**
- 重复的处理逻辑
- 缺少抽象
- 更难扩展
- 表示隐藏类

**重构：**
- 提取类
- 引入参数对象
- 保留整个对象

**示例：**
```javascript
// Data clump: (x, y, z) coordinates
function movePoint(x, y, z, dx, dy, dz) { }
function scalePoint(x, y, z, factor) { }
function distanceBetween(x1, y1, z1, x2, y2, z2) { }

// Extract Point3D class
class Point3D {
  constructor(x, y, z) { }
  move(delta) { }
  scale(factor) { }
  distanceTo(other) { }
}
```
---

## 面向对象的滥用者

气味表明 OOP 原则的使用不完整或不正确。

### Switch 语句

**标志：**
- 长 switch/case 或 if/else 链
- 多个地方相同的开关
- 打开类型代码
- 添加新案例需要到处进行更改

**为什么不好：**
- 违反开闭原则
- 更改所有开关位置的纹波
- 难以扩展
- 通常表明缺少多态性

**重构：**
- 用多态性代替条件式
- 用子类替换类型代码
- 用状态/策略替换类型代码

**示例（之前）：**
```javascript
function calculatePay(employee) {
  switch (employee.type) {
    case 'hourly':
      return employee.hours * employee.rate;
    case 'salaried':
      return employee.salary / 12;
    case 'commissioned':
      return employee.sales * employee.commission;
  }
}
```
**示例（之后）：**
```javascript
class HourlyEmployee {
  calculatePay() {
    return this.hours * this.rate;
  }
}

class SalariedEmployee {
  calculatePay() {
    return this.salary / 12;
  }
}
```
---

### 临时字段

**标志：**
- 实例变量仅在某些方法中使用
- 有条件设置的字段
- 某些情况下的复杂初始化

**为什么不好：**
- 令人困惑——字段存在但可能为空
- 难以理解对象状态
- 表示条件逻辑隐藏

**重构：**
- 提取类
- 引入空对象
- 将临时字段替换为本地

---

### 拒绝遗赠

**标志：**
- 子类不使用继承的方法/数据
- 子类覆盖不执行任何操作
- 继承用于代码重用，而不是 IS-A 关系

**为什么不好：**
- 错误的抽象
- 违反里氏替换原则
- 误导性的等级制度

**重构：**
- 下推方法/字段
- 用委托替换子类
- 用委托代替继承

---

### 具有不同接口的替代类

**标志：**
- 两个做类似事情的类
- 相同概念的不同方法名称
- 可以互换使用

**为什么不好：**
- 重复实施
- 没有通用接口
- 难以切换

**重构：**
- 重命名方法
- 移动方法
- 提取超类
- 提取接口

---

## 变革预防者

气味使改变变得困难——改变一件事需要改变许多其他事情。

### 发散变化

**标志：**
- 一个班级因多种不同原因而发生变化
- 不同区域的变化会触发同类别的编辑
- 班级是“神班级”

**为什么不好：**
- 违反单一责任
- 高变化频率
- 合并冲突

**重构：**
- 提取类
- 提取超类
- 提取子类

**示例：**
`User` 类更改为：
- 身份验证更改
- 个人资料变更
- 账单变更
- 通知变更

→ 摘录：`AuthService`、`ProfileService`、`BillingService`、`NotificationService`

---

### 霰弹枪手术

**标志：**
- 一项更改需要在多个类中进行编辑
- 小功能需要接触 10 个以上文件
- 变化比较分散，很难找到全部

**为什么不好：**
- 容易错过某个地点
- 高耦合
- 更改容易出错

**重构：**
- 移动方法
- 移动领域
- 内联类

**检测：**
查找：添加一个字段需要更改 >5 个文件。

---

### 并行继承层次结构

**标志：**
- 在一个层次结构中创建子类需要在另一层次结构中创建子类
- 类前缀匹配（例如，`DatabaseOrder`、`DatabaseProduct`）

**为什么不好：**
- 双倍维护
- 层次结构之间的耦合
- 容易忘记一侧

**重构：**
- 移动方法
- 移动领域
- 消除一个层次结构

---

## 可有可无

一些不必要的东西应该被删除。

### 评论（过多）

**标志：**
- 注释解释了代码的作用
- 注释掉的代码
- 永远挥之不去的TODO/FIXME
- 在评论中道歉

**为什么不好：**
- 评论撒谎（不同步）
- 代码应该是自我记录的
- 死代码会导致混乱

**重构：**
- 提取方法（名称解释了什么）
- 重命名（清晰无注释）
- 删除注释代码
- 引入断言

**好评论与坏评论：**
```javascript
// BAD: Explaining what
// Loop through users and check if active
for (const user of users) {
  if (user.status === 'active') { }
}

// GOOD: Explaining why
// Active users only - inactive are handled by cleanup job
const activeUsers = users.filter(u => u.isActive);
```
---

### 重复代码

**标志：**
- 多个地方相同的代码
- 类似的代码，但有微小的变化
- 复制粘贴图案

**为什么不好：**
- 多个地方需要修复错误
- 不一致风险
- 臃肿的代码库

**重构：**
- 提取方法
- 提取类
- 上拉方法（在层次结构中）
- 表格模板法

**检测规则：**
任何重复 3 次以上的代码都应该被提取。

---

### 懒惰类

**标志：**
- 阶级不足以证明存在的合理性
- 没有附加值的包装
- 过度设计的结果

**为什么不好：**
- 维护费用
- 不必要的间接
- 复杂而没有好处

**重构：**
- 内联类
- 折叠层次结构

---

### 死代码

**标志：**
- 无法访问的代码
- 未使用的变量/方法/类
- 注释掉的代码
- 不可能条件背后的代码

**为什么不好：**
- 混乱
- 维护负担
- 减慢理解速度

**重构：**
- 删除死代码
- 安全删除

**检测：**
```bash
# Look for unused exports
# Look for unreferenced functions
# IDE "unused" warnings
```
---

### 推测的普遍性

**标志：**
- 具有一个子类的抽象类
- 未使用的参数“以供将来使用”
- 仅委托的方法
- 一个用例的“框架”

**为什么不好：**
- 复杂而没有好处
- YAGNI（你不需要它）
- 较难理解

**重构：**
- 折叠层次结构
- 内联类
- 删除参数
- 重命名方法

---

## 耦合器

代表类之间过度耦合的气味。

### 功能羡慕

**标志：**
- 方法使用来自另一个类的数据多于它自己的数据
- 对另一个对象的许多 getter 调用
- 数据和行为分离

**为什么不好：**
- 错误的行为地点
- 封装不良
- 难以维护

**重构：**
- 移动方法
- 移动领域
- 提取方法（然后移动）

**示例（之前）：**
```javascript
class Order {
  getDiscountedPrice(customer) {
    // Uses customer data heavily
    if (customer.loyaltyYears > 5) {
      return this.price * customer.discountRate;
    }
    return this.price;
  }
}
```
**示例（之后）：**
```javascript
class Customer {
  getDiscountedPriceFor(price) {
    if (this.loyaltyYears > 5) {
      return price * this.discountRate;
    }
    return price;
  }
}
```
---

### 不恰当的亲密行为

**标志：**
- 类可以访问彼此的私有部分
- 双向参考
- 子类对父类了解太多

**为什么不好：**
- 高耦合
- 级联变化
- 很难在没有其他的情况下修改一个

**重构：**
- 移动方法
- 移动领域
- 将双向更改为单向
- 提取类
- 隐藏委托

---

### 消息链

**标志：**
- 长链方法调用：`a.getB().getC().getD().getValue()`
- 客户端取决于导航结构
- “火车失事”代码

**为什么不好：**
- 脆弱——任何改变都会破坏链条
- 违反德墨忒尔法则
- 结构耦合

**重构：**
- 隐藏委托
- 提取方法
- 移动方法

**示例：**
```javascript
// Bad: Message chain
const managerName = employee.getDepartment().getManager().getName();

// Better: Hide delegation
const managerName = employee.getManagerName();
```
---

### 中间人

**标志：**
- 只委托给另一个类
- 一半的方法是委托
- 无附加值

**为什么不好：**
- 不必要的间接
- 维护费用
- 令人困惑的架构

**重构：**
- 删除中间人
- 内联方法

---

## 气味严重程度指南

|严重性 |描述 |行动|
|----------|-------------|--------|
| **关键** |阻碍开发，导致错误 |立即修复 |
| **高** |巨大的维护负担|在当前冲刺中修复 |
| **中** |引人注目但易于管理|近期计划|
| **低** |轻微不便 |趁机修复|

---

## 快速检测清单

扫描代码时使用此清单：

- [ ] 有超过 30 行的方法吗？
- [ ] 任何类 > 300 行？
- [ ] 有超过 4 个参数的方法吗？
- [ ] 有重复的代码块吗？
- [ ] 类型代码上有开关/外壳吗？
- [ ] 有未使用的代码吗？
- [ ] 有没有大量使用另一个类的数据的方法？
- [ ] 是否有长链方法调用？
- [ ] 有任何评论解释“什么”而不是“为什么”吗？
- [ ] 任何应该是对象的原语？

---

## 进一步阅读

-福勒，M.（2018）。 *重构：改进现有代码的设计*（第二版）
- Kerievsky, J. (2004)。 *重构模式*
- Feathers, M. (2004)。 *有效地处理遗留代码*
