# 重构目录

Martin Fowler 的《重构》（第二版）中精选的重构技术目录。每次重构都包括动机、分步机制和示例。

> “重构是由其机制定义的——执行更改时所遵循的精确步骤顺序。” — 马丁·福勒

---

## 如何使用本目录

1. **使用代码气味参考识别气味**
2. **在此目录中找到匹配的重构**
3. **按照机制**一步一步进行
4. **每个步骤后进行测试**以确保保留行为

**黄金法则**：如果任何步骤需要超过 10 分钟，请将其分成更小的步骤。

---

## 最常见的重构

### 提取方法

**何时使用**：长方法、重复代码、需要命名一个概念

**动机**：将代码片段变成一个方法，其名称解释了目的。

**力学**：
1. 创建一个新方法，以它的作用（而不是如何）命名
2.将代码片段复制到新方法中
3. 扫描片段中使用的局部变量
4. 将局部变量作为参数传递（或在方法中声明）
5. 适当处理返回值
6. 用对新方法的调用替换原始片段
7. 测试

**之前**：
```javascript
function printOwing(invoice) {
  let outstanding = 0;

  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");

  // Calculate outstanding
  for (const order of invoice.orders) {
    outstanding += order.amount;
  }

  // Print details
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```
**后**：
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}

function printBanner() {
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
}

function calculateOutstanding(invoice) {
  return invoice.orders.reduce((sum, order) => sum + order.amount, 0);
}

function printDetails(invoice, outstanding) {
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```
---

### 内联方法

**何时使用**：方法体和它的名字一样清晰，过度委托

**动机**：当方法不增加价值时，删除不必要的间接。

**力学**：
1. 检查方法是否是多态的
2.查找所有对该方法的调用
3.将每次调用替换为方法体
4、每次更换后进行测试
5.删除方法定义

**之前**：
```javascript
function getRating(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```
**后**：
```javascript
function getRating(driver) {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```
---

### 提取变量

**何时使用**：难以理解的复杂表达

**动机**：为复杂表达式的一部分命名。

**力学**：
1. 确保表达无副作用
2.声明一个不可变变量
3. 将其设置为表达式（或部分）的结果
4.用变量替换原来的表达式
5. 测试

**之前**：
```javascript
return order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100);
```
**后**：
```javascript
const basePrice = order.quantity * order.itemPrice;
const quantityDiscount = Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
const shipping = Math.min(basePrice * 0.1, 100);
return basePrice - quantityDiscount + shipping;
```
---

### 内联变量

**何时使用**：变量名并不比表达式传达更多信息

**动机**：消除不必要的间接性。

**力学**：
1.检查右侧是否有副作用
2. 如果变量不是不可变的，则使其不变并进行测试
3. 找到第一个引用并替换为表达式
4. 测试
5. 对所有参考文献重复此操作
6.删除声明和赋值
7. 测试

---

### 重命名变量

**何时使用**：名称没有明确传达目的

**动机**：好的名称对于干净的代码至关重要。

**力学**：
1.如果变量使用广泛，考虑封装
2. 查找所有参考文献
3. 更改每个参考
4. 测试

**提示**：
- 使用能透露意图的名称
- 避免缩写
- 使用领域术语
```javascript
// Bad
const d = 30;
const x = users.filter(u => u.a);

// Good
const daysSinceLastLogin = 30;
const activeUsers = users.filter(user => user.isActive);
```
---

### 更改函数声明

**何时使用**：函数名称没有说明用途，参数需要更改

**动机**：好的函数名称使代码能够自我记录。

**力学（简单）**：
1.去掉不需要的参数
2. 更改名称
3.添加需要的参数
4. 测试

**机制（迁移 - 用于复杂的更改）**：
1. 如果删除参数，请确保它没有被使用
2. 使用所需的声明创建新函数
3.让旧函数调用新函数
4. 测试
5.更改调用者以使用新功能
6. 每次之后进行测试
7.删除旧功能

**之前**：
```javascript
function circum(radius) {
  return 2 * Math.PI * radius;
}
```
**后**：
```javascript
function circumference(radius) {
  return 2 * Math.PI * radius;
}
```
---

### 封装变量

**何时使用**：从多个地方直接访问数据

**动机**：为数据操作提供清晰的访问点。

**力学**：
1.创建getter和setter函数
2. 查找所有参考文献
3.用getter代替reads
4.用setter替换写入
5. 每次更改后进行测试
6.限制变量的可见性

**之前**：
```javascript
let defaultOwner = { firstName: "Martin", lastName: "Fowler" };

// Used in many places
spaceship.owner = defaultOwner;
```
**后**：
```javascript
let defaultOwnerData = { firstName: "Martin", lastName: "Fowler" };

function defaultOwner() { return defaultOwnerData; }
function setDefaultOwner(arg) { defaultOwnerData = arg; }

spaceship.owner = defaultOwner();
```
---

### 引入参数对象

**何时使用**：经常一起使用的几个参数

**动机**：将自然归属的数据分组。

**力学**：
1. 为分组参数创建一个新的类/结构
2. 测试
3. 使用更改函数声明添加新对象
4. 测试
5. 对于组中的每个参数，将其从函数中删除并使用新对象
6. 每次之后进行测试

**之前**：
```javascript
function amountInvoiced(startDate, endDate) { ... }
function amountReceived(startDate, endDate) { ... }
function amountOverdue(startDate, endDate) { ... }
```
**后**：
```javascript
class DateRange {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }
}

function amountInvoiced(dateRange) { ... }
function amountReceived(dateRange) { ... }
function amountOverdue(dateRange) { ... }
```
---

### 将函数合并到类中

**何时使用**：多个函数对同一数据进行操作

**动机**：将函数及其操作的数据分组。

**力学**：
1. 对普通数据应用Encapsulate Record
2.将每个函数移到类中
3.每次移动后进行测试
4. 使用类字段替换数据参数

**之前**：
```javascript
function base(reading) { ... }
function taxableCharge(reading) { ... }
function calculateBaseCharge(reading) { ... }
```
**后**：
```javascript
class Reading {
  constructor(data) { this._data = data; }

  get base() { ... }
  get taxableCharge() { ... }
  get calculateBaseCharge() { ... }
}
```
---

### 分相

**何时使用**：代码处理两个不同的事情

**动机**：将代码分成具有清晰边界的不同阶段。

**力学**：
1.为第二阶段创建第二个函数
2. 测试
3.在阶段之间引入中间数据结构
4. 测试
5. 将第一阶段提取到自己的函数中
6. 测试

**之前**：
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  const shippingPerCase = (basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = quantity * shippingPerCase;
  return basePrice - discount + shippingCost;
}
```
**后**：
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const priceData = calculatePricingData(product, quantity);
  return applyShipping(priceData, shippingMethod);
}

function calculatePricingData(product, quantity) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  return { basePrice, quantity, discount };
}

function applyShipping(priceData, shippingMethod) {
  const shippingPerCase = (priceData.basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = priceData.quantity * shippingPerCase;
  return priceData.basePrice - priceData.discount + shippingCost;
}
```
---

## 移动功能

### 移动方法

**何时使用**：方法使用另一个类的功能多于其自己的功能

**动机**：将函数与最常用的数据放在一起。

**力学**：
1.检查其类中的方法使用的所有程序元素
2. 检查方法是否多态
3. 将方法复制到目标类
4.适应新环境
5.将原始方法委托给目标
6. 测试
7.考虑删除原来的方法

---

### 移动字段

**何时使用**：字段更多地被另一个类使用

**动机**：将数据与使用它的函数一起保存。

**力学**：
1. 封装该字段（如果尚未封装）
2. 测试
3. 在目标中创建字段
4. 更新引用以使用目标字段
5. 测试
6.删除原来的字段

---

### 将语句移至函数中

**何时使用**：相同的代码总是与函数调用一起出现

**动机**：通过将重复的代码移动到函数中来消除重复。

**力学**：
1. 如果还没有将重复的代码提取到函数中
2. 将语句移至该函数中
3. 测试
4. 如果调用者不再需要独立语句，请将其删除

---

### 将语句移至调用者

**何时使用**：调用者之间的常见行为有所不同

**动机**：当行为需要不同时，将其移出功能。

**力学**：
1.对代码使用Extract Method进行移动
2.在原函数上使用Inline Method
3. 删除现在内联的调用
4. 将提取的代码移至每个调用者
5. 测试

---

## 组织数据

### 用对象替换原语

**何时使用**：数据项需要比简单值更多的行为

**动机**：将数据及其行为封装起来。

**力学**：
1.应用封装变量
2. 创建一个简单的值类
3.更改setter来创建新实例
4.更改getter返回值
5. 测试
6.为新类添加更丰富的行为

**之前**：
```javascript
class Order {
  constructor(data) {
    this.priority = data.priority; // string: "high", "rush", etc.
  }
}

// Usage
if (order.priority === "high" || order.priority === "rush") { ... }
```
**后**：
```javascript
class Priority {
  constructor(value) {
    if (!Priority.legalValues().includes(value))
      throw new Error(`Invalid priority: ${value}`);
    this._value = value;
  }

  static legalValues() { return ['low', 'normal', 'high', 'rush']; }
  get value() { return this._value; }

  higherThan(other) {
    return Priority.legalValues().indexOf(this._value) >
           Priority.legalValues().indexOf(other._value);
  }
}

// Usage
if (order.priority.higherThan(new Priority("normal"))) { ... }
```
---

### 将 Temp 替换为 Query

**何时使用**：临时变量保存表达式的结果

**动机**：通过将表达式提取到函数中使代码更清晰。

**力学**：
1. 检查变量是否只被赋值一次
2. 将赋值的右侧提取到方法中
3. 用方法调用替换对 temp 的引用
4. 测试
5. 删除临时声明和赋值

**之前**：
```javascript
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000) {
  return basePrice * 0.95;
} else {
  return basePrice * 0.98;
}
```
**后**：
```javascript
get basePrice() {
  return this._quantity * this._itemPrice;
}

// In the method
if (this.basePrice > 1000) {
  return this.basePrice * 0.95;
} else {
  return this.basePrice * 0.98;
}
```
---

## 简化条件逻辑

### 条件分解

**何时使用**：复杂条件 (if-then-else) 语句

**动机**：通过提取条件和行动来明确意图。

**力学**：
1. 对条件应用提取方法
2. 在then-branch上应用Extract Method
3. 在 else 分支上应用提取方法（如果存在）

**之前**：
```javascript
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)) {
  charge = quantity * plan.summerRate;
} else {
  charge = quantity * plan.regularRate + plan.regularServiceCharge;
}
```
**后**：
```javascript
if (isSummer(aDate, plan)) {
  charge = summerCharge(quantity, plan);
} else {
  charge = regularCharge(quantity, plan);
}

function isSummer(date, plan) {
  return !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
}

function summerCharge(quantity, plan) {
  return quantity * plan.summerRate;
}

function regularCharge(quantity, plan) {
  return quantity * plan.regularRate + plan.regularServiceCharge;
}
```
---

### 巩固条件表达式

**何时使用**：具有相同结果的多个条件

**动机**：明确条件是单次检查。

**力学**：
1. 验证条件下无副作用
2. 使用 `and` 或 `or` 组合条件
3. 结合条件考虑Extract Method

**之前**：
```javascript
if (employee.seniority < 2) return 0;
if (employee.monthsDisabled > 12) return 0;
if (employee.isPartTime) return 0;
```
**后**：
```javascript
if (isNotEligibleForDisability(employee)) return 0;

function isNotEligibleForDisability(employee) {
  return employee.seniority < 2 ||
         employee.monthsDisabled > 12 ||
         employee.isPartTime;
}
```
---

### 用保护子句替换嵌套条件

**何时使用**：深度嵌套的条件使得流程难以遵循

**动机**：对特殊情况使用保护条款，保持正常流程清晰。

**力学**：
1. 查找特殊情况条件
2. 用提前返回的保护子句替换它们
3.每次更改后进行测试

**之前**：
```javascript
function payAmount(employee) {
  let result;
  if (employee.isSeparated) {
    result = { amount: 0, reasonCode: "SEP" };
  } else {
    if (employee.isRetired) {
      result = { amount: 0, reasonCode: "RET" };
    } else {
      result = calculateNormalPay(employee);
    }
  }
  return result;
}
```
**后**：
```javascript
function payAmount(employee) {
  if (employee.isSeparated) return { amount: 0, reasonCode: "SEP" };
  if (employee.isRetired) return { amount: 0, reasonCode: "RET" };
  return calculateNormalPay(employee);
}
```
---

### 用多态替换条件

**何时使用**：基于类型的 Switch/case，条件逻辑因类型而异

**动机**：让对象处理自己的行为。

**力学**：
1. 创建类层次结构（如果不存在）
2.使用Factory Function进行对象创建
3. 将条件逻辑移至超类方法中
4. 为每种情况创建子类方法
5.删除原来的条件

**之前**：
```javascript
function plumages(birds) {
  return birds.map(b => plumage(b));
}

function plumage(bird) {
  switch (bird.type) {
    case 'EuropeanSwallow':
      return "average";
    case 'AfricanSwallow':
      return (bird.numberOfCoconuts > 2) ? "tired" : "average";
    case 'NorwegianBlueParrot':
      return (bird.voltage > 100) ? "scorched" : "beautiful";
    default:
      return "unknown";
  }
}
```
**后**：
```javascript
class Bird {
  get plumage() { return "unknown"; }
}

class EuropeanSwallow extends Bird {
  get plumage() { return "average"; }
}

class AfricanSwallow extends Bird {
  get plumage() {
    return (this.numberOfCoconuts > 2) ? "tired" : "average";
  }
}

class NorwegianBlueParrot extends Bird {
  get plumage() {
    return (this.voltage > 100) ? "scorched" : "beautiful";
  }
}

function createBird(data) {
  switch (data.type) {
    case 'EuropeanSwallow': return new EuropeanSwallow(data);
    case 'AfricanSwallow': return new AfricanSwallow(data);
    case 'NorwegianBlueParrot': return new NorwegianBlueParrot(data);
    default: return new Bird(data);
  }
}
```
---

### 引入特殊情况（空对象）

**何时使用**：特殊情况下重复空检查

**动机**：返回一个处理特殊情况的特殊对象。

**力学**：
1. 创建具有预期接口的特殊案例类
2.添加isSpecialCase检查
3.引入工厂方法
4. 用特殊情况对象用法替换空检查
5. 测试

**之前**：
```javascript
const customer = site.customer;
// ... many places checking
if (customer === "unknown") {
  customerName = "occupant";
} else {
  customerName = customer.name;
}
```
**后**：
```javascript
class UnknownCustomer {
  get name() { return "occupant"; }
  get billingPlan() { return registry.defaultPlan; }
}

// Factory method
function customer(site) {
  return site.customer === "unknown"
    ? new UnknownCustomer()
    : site.customer;
}

// Usage - no null checks needed
const customerName = customer.name;
```
---

## 重构 API

### 将查询与修饰符分开

**何时使用**：函数既返回值又具有副作用

**动机**：明确哪些操作有副作用。

**力学**：
1.新建查询函数
2.复制原函数的返回逻辑
3.修改原来的返回void
4. 替换使用返回值的调用
5. 测试

**之前**：
```javascript
function alertForMiscreant(people) {
  for (const p of people) {
    if (p === "Don") {
      setOffAlarms();
      return "Don";
    }
    if (p === "John") {
      setOffAlarms();
      return "John";
    }
  }
  return "";
}
```
**后**：
```javascript
function findMiscreant(people) {
  for (const p of people) {
    if (p === "Don") return "Don";
    if (p === "John") return "John";
  }
  return "";
}

function alertForMiscreant(people) {
  if (findMiscreant(people) !== "") setOffAlarms();
}
```
---

### 参数化函数

**何时使用**：多个函数使用不同的值执行类似的操作

**动机**：通过添加参数来删除重复。

**力学**：
1. 选择一项功能
2. 为变化的文字添加参数
3.更改body以使用参数
4. 测试
5. 更改调用者以使用参数化版本
6.删除现在不使用的功能

**之前**：
```javascript
function tenPercentRaise(person) {
  person.salary = person.salary * 1.10;
}

function fivePercentRaise(person) {
  person.salary = person.salary * 1.05;
}
```
**后**：
```javascript
function raise(person, factor) {
  person.salary = person.salary * (1 + factor);
}

// Usage
raise(person, 0.10);
raise(person, 0.05);
```
---

### 删除标志参数

**何时使用**：改变函数行为的布尔参数

**动机**：通过单独的功能使行为明确。

**力学**：
1.为每个标志值创建显式函数
2. 用适当的新函数替换每个调用
3.每次更改后进行测试
4.删除原有功能

**之前**：
```javascript
function bookConcert(customer, isPremium) {
  if (isPremium) {
    // premium booking logic
  } else {
    // regular booking logic
  }
}

bookConcert(customer, true);
bookConcert(customer, false);
```
**后**：
```javascript
function bookPremiumConcert(customer) {
  // premium booking logic
}

function bookRegularConcert(customer) {
  // regular booking logic
}

bookPremiumConcert(customer);
bookRegularConcert(customer);
```
---

## 处理继承

### 上拉方法

**何时使用**：多个子类中相同的方法

**动机**：删除类层次结构中的重复项。

**力学**：
1. 检查方法以确保它们相同
2.检查签名是否相同
3. 在超类中创建新方法
4. 从一个子类复制主体
5.删除一个子类方法，测试
6.删除其他子类方法，分别测试

---

### 下推法

**何时使用**：仅与子类的子集相关的行为

**动机**：将方法放在使用的地方。

**力学**：
1.将方法复制到每个需要它的子类中
2. 从超类中删除方法
3. 测试
4.从不需要的子类中删除
5. 测试

---

### 用委托替换子类

**何时使用**：继承被错误地使用，需要更多的灵活性

**动机**：在适当的情况下，更喜欢组合而不是继承。

**力学**：
1.为委托创建空类
2.向持有委托的宿主类添加字段
3. 为委托创建构造函数，从主机调用
4. 将功能移至委托
5. 每次移动后进行测试
6. 用委托代替继承

---

## 提取类

**何时使用**：具有多重职责的大类

**动机**：拆分班级以保持单一责任。

**力学**：
1. 决定如何划分职责
2.创建新类
3. 将字段从原来的类移至新类
4. 测试
5. 将方法从原始类移至新类
6. 每次移动后进行测试
7. 检查并重命名两个类
8. 决定如何公开新类

**之前**：
```javascript
class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get officeAreaCode() { return this._officeAreaCode; }
  set officeAreaCode(arg) { this._officeAreaCode = arg; }
  get officeNumber() { return this._officeNumber; }
  set officeNumber(arg) { this._officeNumber = arg; }

  get telephoneNumber() {
    return `(${this._officeAreaCode}) ${this._officeNumber}`;
  }
}
```
**后**：
```javascript
class Person {
  constructor() {
    this._telephoneNumber = new TelephoneNumber();
  }
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get telephoneNumber() { return this._telephoneNumber.toString(); }
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  set officeAreaCode(arg) { this._telephoneNumber.areaCode = arg; }
}

class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  set areaCode(arg) { this._areaCode = arg; }
  get number() { return this._number; }
  set number(arg) { this._number = arg; }
  toString() { return `(${this._areaCode}) ${this._number}`; }
}
```
---

## 快速参考：重构的味道

|代码气味 |初级重构 |另类|
|------------|--------------------|-------------|
|长方法 |提取方法|将 Temp 替换为 Query |
|重复代码 |提取方法|上拉法 |
|大班|提取类 |提取子类|
|长参数列表 |引入参数对象 |保留整个对象|
|功能羡慕|移动方法|提取方法+移动|
|数据块|提取类 |引入参数对象 |
|原始的痴迷|用对象替换原语 |替换类型代码 |
| Switch 语句 |用多态性代替条件式 |替换类型代码 |
|临时场地|提取类 |引入空对象|
|消息链|隐藏委托 |提取方法|
|中间人 |删除中间人 |内联方法|
|发散变化 |提取类 |分相|
|霰弹枪手术 |移动方法|内联类 |
|死代码 |删除死代码 | - |
|推测的普遍性|折叠层次结构|内联类 |

---

## 进一步阅读

-福勒，M.（2018）。 *重构：改进现有代码的设计*（第二版）
- 在线目录：https://refactoring.com/catalog/
