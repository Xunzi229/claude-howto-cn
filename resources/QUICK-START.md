# 快速入门 - 品牌资产

## 将资源复制到您的项目
```bash
# Copy all resources to your web project
cp -r resources/ /path/to/your/website/

# Or just the favicons for web
cp resources/favicons/* /path/to/your/website/public/
```
## 添加到 HTML（复制和粘贴）
```html
<!-- Favicons -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
<meta name="theme-color" content="#000000">
```
## 在 Markdown/文档中使用
```markdown
# Claude How To

![Claude How To Logo](resources/logos/claude-howto-logo.svg)

![Icon](resources/icons/claude-howto-icon.svg)
```
## 推荐尺寸

|目的|尺寸|文件|
|---------|------|------|
|网站标题| 520×120 | `logos/claude-howto-logo.svg` |
|应用程序图标| 256×256 | 256×256 `icons/claude-howto-icon.svg` |
|浏览器选项卡 | 32×32 | 32×32 `favicons/favicon-32.svg` |
|手机主屏| 128×128 | 128×128 `favicons/favicon-128.svg` |
|桌面应用程序 | 256×256 | 256×256 `favicons/favicon-256.svg` |
|小头像 | 64×64 | 64×64 `favicons/favicon-64.svg` |

## 颜色值
```css
/* Use these in your CSS */
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```
## 图标设计意义

**带代码支架的指南针**：
- 指南针环 = 导航、结构化学习路径
- 绿色北针=方向、进步、指导
- 黑色南针 = 接地，基础扎实
- `>` 括号 = 终端提示、代码、CLI 上下文
- 刻度线 = 精确、结构化的步骤

这象征着“在明确的指导下通过代码找到自己的方式”。

## 使用什么、在哪里使用

### 网站
- **标题**：徽标 (`logos/claude-howto-logo.svg`)
- **网站图标**：32 像素 (`favicons/favicon-32.svg`)
- **社交预览**：图标 (`icons/claude-howto-icon.svg`)

### GitHub
- **自述文件徽章**：图标 (`icons/claude-howto-icon.svg`) 位于 64-128 像素
- **存储库头像**：图标 (`icons/claude-howto-icon.svg`)

### 社交媒体
- **个人资料图片**：图标 (`icons/claude-howto-icon.svg`)
- **横幅**：徽标 (`logos/claude-howto-logo.svg`)
- **缩略图**：图标尺寸为 256×256px

### 文档
- **章节标题**：徽标或图标（缩放以适合）
- **导航图标**：Favicon (32-64px)

---

有关完整文档，请参阅 [README.md](README.md)。
