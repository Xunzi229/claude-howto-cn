# Claude How To - 设计系统

## 视觉识别

### 图标设计理念：带代码括号的指南针

Claude How To 图标使用 **带有 `>` 代码括号**的指南针来表示通过代码的引导导航：
```
     N (green)
     ▲
     │
W ───>─── E     Compass = Guidance/Direction
     │          > Bracket = Code/Terminal/CLI
     ▼
     S (black)
```
这将创建：
- **视觉清晰度**：立即传达“代码导航指南”
- **象征意义**：指南针=找到你的路； `>` = 代码/终端
- **可扩展性**：适用于从 16 像素到 512 像素的任何尺寸
- **品牌对齐**：以最少的调色板匹配开发人员工具的美感

---

## 颜色系统

### 调色板

|颜色 |十六进制 | RGB |用途 |
|--------|-----|-----|--------|
|黑色（主要）| `#000000` | 0, 0, 0 |主笔、文字、南针|
|白色（背景）| `#FFFFFF` | 255, 255, 255 | 255, 255, 255浅色背景|
|灰色（中学）| `#6B7280` | 107、114、128 |小刻度线，辅助文本 |
|亮绿色（强调色）| `#22C55E` | 34、197、94 |北针、中心点、强调线|
|近黑色（深色 BG）| `#0A0A0A` | 10, 10, 10 | 10, 10, 10 |深色模式背景 |

### 对比度 (WCAG)

- 白底黑字：**21:1** AAA
- 白底灰：**4.6:1** AA
- 白底绿：**3.2:1**（仅用于装饰，不适用于文本）
- 黑底白字：**19.5:1** AAA

### 强调色规则

**亮绿色 (#22C55E) 仅供亮点使用：**
- 指南针北针
- 中心点
- 重音下划线/边框
- 切勿作为背景颜色
- 切勿用于正文

---

## 版式

### 标志字体
- **系列**：Inter、SF Pro Display、-apple-system、Segoe UI、sans-serif
- **“Claude”**：42 像素，粗细 700（粗体），黑色
- **“操作方法”**：32 像素，重量 500（中），灰色 (#6B7280)
- **副标题**：10px，粗细500，灰色，字母间距1.5px，大写

### 界面字体
- **系列**：Inter、SF Pro、系统字体（无衬线字体）
- **重量**：400-600
- **风格**：干净、可读

---

## 图标详细信息

### 指南针规格

罗盘标记是由这些几何元素构成的：
```
Element             | Stroke/Fill    | Color
--------------------|----------------|------------------
Outer ring          | 3px stroke     | Black / White (dark mode)
North tick          | 2.5px stroke   | Black / White (dark mode)
Other cardinal ticks| 2px stroke     | Gray / White 50% (dark mode)
Intercardinal ticks | 1.5px stroke   | Gray / White 40% (dark mode)
North needle        | filled polygon | #22C55E (always green)
South needle        | filled polygon | Black / White (dark mode)
> bracket           | 3px stroke     | Black / White (dark mode)
Center dot          | filled circle  | #22C55E (always green)
```
### 尺寸进展
```
16px  → Ring + needles + chevron only (minimal)
32px  → Adds cardinal tick marks
64px  → Adds intercardinal tick marks
128px → Full detail, all elements crisp
256px → Maximum detail, thick strokes
```
---

## 尺码指南

### 徽标尺寸

- **最小**：200px 宽度（用于网页）
- **推荐**：520px（原始尺寸）
- **最大**：无限制（矢量格式）
- **纵横比**：~4.3:1（宽度：高度）

### 图标大小调整

- **最小**：16px（图标）
- **推荐**：64-256px（应用程序、头像）
- **最大**：无限制（矢量格式）
- **纵横比**：1:1（正方形）

---

## 间距和对齐方式

### 徽标间距
```
┌─────────────────────────────────────┐
│                                     │
│        Clear Space Minimum          │
│         (logo height / 2)           │
│                                     │
│    [COMPASS]  Claude                │
│               How-To                │
│                                     │
└─────────────────────────────────────┘
```
### 图标中心点

所有图标都以画布的中点为中心：
- 256px 画布为 128×128
- 128px 画布为 64×64
- 与其他 UI 元素保持一致

---

## 辅助功能

### 颜色对比
- 所有文本均符合 WCAG AA（最低 4.5:1）
- 绿色口音是装饰性的，而不是信息性的
- 无红绿颜色依赖性

### 可扩展性
- 矢量格式确保任何尺寸的清晰度
- 几何形状在 16 像素时仍可识别
- 基于可用尺寸的渐进式细节

---

## 应用实例

### 网页标题
- 尺寸：520×120px 标志
- 文件：`logos/claude-howto-logo.svg`
- 背景：白色或深色 (#0A0A0A)
- 内边距：最小 20 像素

### 应用程序图标
- 尺寸：256×256像素
- 文件：`icons/claude-howto-icon.svg`
- 背景：白色或深色
- 使用：应用程序快捷方式、头像

### 浏览器图标
- 大小：32px（主要），16px（后备）
- 文件：`favicons/favicon-32.svg`
- 格式：SVG 清晰显示

### 社交媒体
- 个人资料：256×256px 图标
- 横幅：520×120px 徽标（居中）

### 文档
- 章节标题：徽标缩放以适合
- 部分图标：64×64px 图标
- 内联：32×32px 图标

---

## 文件格式详细信息

### SVG 结构

所有 SVG 文件都是平面设计：
- 无渐变（仅限纯色）
- 无滤镜效果（无模糊、发光或阴影）
- 清洁描边和填充几何形状
- 用于响应式缩放的 ViewBox
- 可读的、带注释的代码

### 跨浏览器兼容性

- Chrome/Edge：完全支持
- 火狐浏览器：完全支持
- Safari：完全支持
- iOS Safari：完全支持
- 所有现代浏览器：完全支持

---

## 定制

### 更改强调色

要创建具有不同口音的变体：

1. 将 `#22C55E` 的所有实例替换为您的强调色
2. 确保装饰元素的对比度保持在 3:1 以上
3. 保持黑/白/灰结构不变

### 缩放
```css
svg {
  width: 256px;
  height: 256px;
}
```
SVG 通过 viewBox 自动缩放 - 无需转换。

---

## 版本控制

在 git 中跟踪设计变更：
- 正常版本 SVG 文件（它们是文本）
- 带有设计变更的标签发布
- 在提交中包含 DESIGN-SYSTEM.md

---

**最后更新**：2026 年 2 月
**设计系统版本**：3.0
