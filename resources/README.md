<picture>
  <source media="(prefers-color-scheme: dark)" srcset="logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="logos/claude-howto-logo.svg">
</picture>

# claude如何 - 品牌资产

Claude How To 项目的完整徽标、图标和网站图标集合。所有资源均使用 V3.0 设计：带有代码括号 (`>`) 符号的指南针，代表通过代码进行引导导航 - 使用带有亮绿色 (#22C55E) 口音的黑/白/灰色调色板。

## 目录结构
```
resources/
├── logos/
│   ├── claude-howto-logo.svg       # Main logo - Light mode (520×120px)
│   └── claude-howto-logo-dark.svg  # Main logo - Dark mode (520×120px)
├── icons/
│   ├── claude-howto-icon.svg       # App icon - Light mode (256×256px)
│   └── claude-howto-icon-dark.svg  # App icon - Dark mode (256×256px)
└── favicons/
    ├── favicon-16.svg              # Favicon - 16×16px
    ├── favicon-32.svg              # Favicon - 32×32px (primary)
    ├── favicon-64.svg              # Favicon - 64×64px
    ├── favicon-128.svg             # Favicon - 128×128px
    └── favicon-256.svg             # Favicon - 256×256px
```
`assets/logo/` 中的其他资产：
```
assets/logo/
├── logo-full.svg       # Mark + wordmark (horizontal)
├── logo-mark.svg       # Compass symbol only (120×120px)
├── logo-wordmark.svg   # Text only
├── logo-icon.svg       # App icon (512×512, rounded)
├── favicon.svg         # 16×16 optimized
├── logo-white.svg      # White version for dark backgrounds
└── logo-black.svg      # Black monochrome version
```
## 资产概览

###设计理念（V3.0）

**带代码支架的指南针** — 指南符合代码：
- **罗盘环** = 导航，找到方向
- **北针（绿色）** = 方向、学习道路上的进展
- **南针（黑色）** = 接地，坚实的基础
- **`>` 括号** = 终端提示、代码、CLI 上下文
- **刻度线** = 精确、结构化的学习

### 标志

**文件**：
- `logos/claude-howto-logo.svg`（灯光模式）
- `logos/claude-howto-logo-dark.svg`（深色模式）

**规格**：
- **尺寸**：520×120 像素
- **目的**：主标题/带有文字标记的品牌徽标
- **用法**：
  - 网站标题
  - 自述文件徽章
  - 营销材料
  - 印刷材料
- **格式**：SVG（完全可扩展）
- **模式**：浅色（白色背景）和深色（#0A0A0A 背景）

### 图标

**文件**：
- `icons/claude-howto-icon.svg`（灯光模式）
- `icons/claude-howto-icon-dark.svg`（深色模式）

**规格**：
- **尺寸**：256×256 像素
- **用途**：应用程序图标、头像、缩略图
- **用法**：
  - 应用程序图标
  - 个人资料头像
  - 社交媒体缩略图
  - 文档标题
- **格式**：SVG（完全可扩展）
- **模式**：浅色（白色背景）和深色（#0A0A0A 背景）

**设计元素**：
- 罗盘环，带有基数和基间刻度线
- 绿色北针（方向/指导）
- 黑南针（粉底）
- `>` 代码括号位于中心（终端/CLI）
- 绿色中心点强调

### 网站图标

多种尺寸的优化版本，适合网络使用：

|文件|尺寸|深度PI |用途 |
|------|------|-----|--------|
| `favicon-16.svg` | 16×16 像素 | 1x |浏览器选项卡（旧版浏览器）|
| `favicon-32.svg` | 32×32 像素 | 1x |标准浏览器图标 |
| `favicon-64.svg` | 64×64 像素 | 1x-2x |高 DPI 显示器 |
| `favicon-128.svg` | 128×128 像素 | 2x |苹果触摸图标，书签|
| `favicon-256.svg` | 256×256 像素 | 4x |现代浏览器，PWA 图标 |

**优化说明**：
- 16px：最小几何形状 - 仅环、针、V 形
- 32px：添加基本刻度线
- 64px+：带有基间刻度的完整细节
- 全部与主图标保持视觉一致性
- SVG 格式确保任何尺寸的清晰显示

## HTML 集成

### 基本网站图标设置
```html
<!-- Browser favicon -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

<!-- Apple touch icon (mobile home screen) -->
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

<!-- PWA & modern browsers -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
```
### 完成设置
```html
<head>
  <!-- Primary favicon -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

  <!-- Apple touch icon -->
  <link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

  <!-- PWA icons -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">

  <!-- Android -->
  <link rel="shortcut icon" href="/resources/favicons/favicon-256.svg">

  <!-- PWA manifest reference (if using manifest.json) -->
  <meta name="theme-color" content="#000000">
</head>
```
## 调色板

### 原色
- **黑色**：`#000000`（主要文字、笔画、南针）
- **白色**：`#FFFFFF`（浅色背景）
- **灰色**：`#6B7280`（辅助文本，小刻度线）

### 强调色
- **亮绿色**：`#22C55E`（北针、中心点、强调线 - 仅突出显示，从不作为背景）

### 深色模式
- **背景**：`#0A0A0A`（近黑色）

### CSS 变量
```css
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```
### Tailwind 配置
```js
colors: {
  brand: {
    primary: '#000000',
    secondary: '#6B7280',
    accent: '#22C55E',
  }
}
```
### 使用指南
- 使用黑色作为主要文本和结构元素
- 使用灰色作为次要/支撑元素
- **仅**使用绿色作为亮点 - 针、点、强调线
- 切勿使用绿色作为背景色
- 保持 WCAG AA 对比度（最低 4.5:1）

## 设计指南

### 标志使用
- 在白色或深色 (#0A0A0A) 背景上使用
- 按比例缩放
- 包括徽标周围的净空间（最小值：徽标高度/2）
- 使用提供的浅色/深色变体以获得适当的背景

### 图标使用
- 以标准尺寸使用：16、32、64、128、256px
- 保持罗盘比例
- 按比例缩放

### 网站图标用法
- 根据上下文使用适当的大小
- 16-32px：浏览器选项卡、书签
- 64px：Favicon 网站图标
- 128px+：Apple/Android 主屏幕

## SVG 优化

所有 SVG 文件都是平面设计，没有渐变或滤镜：
- 干净的基于笔划的几何形状
- 没有嵌入光栅
- 优化路径
- 响应式viewBox

对于网页优化：
```bash
# Compress SVG while maintaining quality
svgo --config='{
  "js2svg": {
    "indent": 2
  },
  "plugins": [
    "convertStyleToAttrs",
    "removeRasterImages"
  ]
}' input.svg -o output.svg
```
## PNG 转换

要将 SVG 转换为 PNG 以支持旧版浏览器：
```bash
# Using ImageMagick
convert -density 300 -background none favicon-256.svg favicon-256.png

# Using Inkscape
inkscape -D -z --file=favicon-256.svg --export-png=favicon-256.png
```
## 辅助功能

- 高对比度色彩比（符合 WCAG AA 标准 — 最低 4.5:1）
- 各种尺寸均可识别的简洁几何形状
- 可扩展的矢量格式
- 图标中没有文本（在文字标记中单独添加文本）
- 含义不依赖红绿颜色

## 归因

这些资产是 Claude How To 项目的一部分。

**许可证**：MIT（参见项目许可证文件）

## 版本历史

- **v3.0**（2026 年 2 月）：罗盘支架设计，带黑/白/灰 + 绿色调色板
- **v2.0**（2026 年 1 月）：claude风格的 12 射线星爆设计，带有祖母绿调色板
- **v1.0**（2026 年 1 月）：原始的基于六边形的进度图标设计

---

**最后更新**：2026 年 2 月
**当前版本**：3.0（指南针支架）
**所有资产**：生产就绪的 SVG、完全可扩展、可访问 WCAG AA
