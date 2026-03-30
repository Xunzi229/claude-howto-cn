---
名称：博客草稿
描述：根据想法和资源起草一篇博客文章。当用户想要撰写博客文章、通过研究创建内容或起草文章时使用。通过版本控制指导研究、头脑风暴、提纲和迭代起草。
---

## 用户输入
```text
$ARGUMENTS
```
在继续之前，您**必须**考虑用户输入。用户应提供：
- **想法/主题**：博客文章的主要概念或主题
- **资源**：URL、文件或研究参考（可选但推荐）
- **目标受众**：博客文章的目标受众（可选）
- **语气/风格**：正式、休闲、技术等（可选）

**重要**：如果用户请求更新**现有博客文章**，请跳过步骤 0-8，直接从 **步骤 9** 开始。首先读取现有的草稿文件，然后继续迭代过程。

## 执行流程

按顺序执行这些步骤。 **请勿跳过步骤或在未经用户批准的情况下继续操作。**

### 第0步：创建项目文件夹

1. 使用格式生成文件夹名称：`YYYY-MM-DD-short-topic-name`
   - 使用今天的日期
- 根据主题创建一个简短的、URL 友好的 slug（小写字母、连字符、最多 5 个单词）

2.创建文件夹结构：
   ```
   blog-posts/
   └── YYYY-MM-DD-short-topic-name/
       └── resources/
   ```
3. 在继续之前与用户确认文件夹创建。

### 第 1 步：研究和资源收集

1.在博文目录下创建`resources/`子文件夹

2. 对于每个提供的资源：
- **URL**：获取关键信息并将其作为 Markdown 文件保存到 `resources/`
- **文件**：阅读并总结 `resources/`
   - **主题**：使用网络搜索来收集最新信息

3. 对于每个资源，在 `resources/` 中创建一个摘要文件：
- `resources/source-1-[short-name].md`
- `resources/source-2-[short-name].md`
   - 等

4. 每份摘要应包括：
   ```markdown
   # Source: [Title/URL]

   ## Key Points
   - Point 1
   - Point 2

   ## Relevant Quotes/Data
   - Quote or statistic 1
   - Quote or statistic 2

   ## How This Relates to Topic
   Brief explanation of relevance
   ```
5. 向用户呈现研究总结。

### 第 2 步：集思广益并澄清

1. 根据想法和研究资源，提出：
   - **从研究中确定的主题**
   - **博客文章的潜在角度**
   - **应涵盖的要点**
   - 需要澄清的信息中的**差距**

2. 提出澄清问题：
   - 您希望读者获得的主要收获是什么？
   - 您想强调研究中的具体要点吗？
   - 目标长度是多少？ （短：500-800字，中：1000-1500，长：2000+）
   - 您想排除任何点吗？

3. **等待用户响应后再继续。**

### 第 3 步：提出大纲

1. 创建结构化大纲，包括：
   ```markdown
   # Blog Post Outline: [Title]

   ## Meta Information
   - **Target Audience**: [who]
   - **Tone**: [style]
   - **Target Length**: [word count]
   - **Main Takeaway**: [key message]

   ## Proposed Structure

   ### Hook/Introduction
   - Opening hook idea
   - Context setting
   - Thesis statement

   ### Section 1: [Title]
   - Key point A
   - Key point B
   - Supporting evidence from [source]

   ### Section 2: [Title]
   - Key point A
   - Key point B

   [Continue for all sections...]

   ### Conclusion
   - Summary of key points
   - Call to action or final thought

   ## Sources to Cite
   - Source 1
   - Source 2
   ```
2. 向用户展示大纲并**请求批准或修改**。

### 第 4 步：保存批准的大纲

1. 用户批准大纲后，将其保存到博客文章文件夹中的 `OUTLINE.md` 中。

2. 确认大纲已保存。

### 步骤 5：提交大纲（如果在 git 存储库中）

1. 检查当前目录是否是git仓库。

2. 如果是：
- 暂存新文件：博客文章文件夹、资源和 OUTLINE.md
- 创建带有消息的提交：`docs: Add outline for blog post - [topic-name]`
   - 推送到远程

3. 如果不是 git repo，请跳过此步骤并通知用户。

### 第 6 步：写草稿

1. 根据批准的大纲，撰写完整的博客文章草稿。

2. 严格遵循 OUTLINE.md 中的结构。

3. 包括：
   - 带有Hook的引人入胜的介绍
   - 清除章节标题
   - 研究的支持证据和例子
   - 各部分之间的平滑过渡
   - 强有力的结论与外卖
   - **引文**：所有比较、统计数据、数据点和事实主张必须引用原始来源

4. 将草稿保存为博客文章文件夹中的 `draft-v0.1.md`。

5. 格式：
   ```markdown
   # [Blog Post Title]

   *[Optional: subtitle or tagline]*

   [Full content with inline citations...]

   ---

   ## References
   - [1] Source 1 Title - URL or Citation
   - [2] Source 2 Title - URL or Citation
   - [3] Source 3 Title - URL or Citation
   ```
6. **引文要求**：
   - 每个数据点、统计数据或比较都必须有内联引用
   - 使用编号参考文献 [1]、[2] 等，或命名引文 [来源名称]
   - 将引用链接到最后的参考文献部分
- 示例：“研究表明 65% 的开发人员更喜欢 TypeScript [1]”
- 示例：“React 在渲染速度方面优于 Vue 20% [React Benchmarks 2024]”

### 第 7 步：提交草稿（如果在 git 存储库中）

1.检查是否在git仓库中。

2. 如果是：
   - 暂存草稿文件
- 创建带有消息的提交：`docs: Add draft v0.1 for blog post - [topic-name]`
   - 推送到远程

3. 如果不是 git repo，则跳过并通知用户。

### 第 8 步：提交草稿供审核

1. 将草稿内容呈现给用户。

2. 寻求反馈：
   - 总体印象？
   - 需要扩大或减少的部分？
   - 需要调整音调吗？
   - 缺少信息？
   - 具体编辑或重写？

3. **等待用户响应。**

### 第 9 步：迭代或最终确定

**如果用户请求更改：**
1. 记下所有要求的修改
2. 返回步骤 6，进行以下调整：
- 增加版本号（v0.2、v0.3等）
   - 纳入所有反馈
- 另存为 `draft-v[X.Y].md`
   - 重复步骤 7-8

**如果用户批准：**
1. 确认最终草案版本
2. 如果用户请求，可以选择重命名为 `final.md`
3.总结一下博文的创作过程：
   - 创建的总版本
   - 版本之间的关键变化
   - 最终字数统计
   - 创建的文件

## 版本跟踪

所有草稿均通过增量版本控制保留：
- `draft-v0.1.md` - 初稿
- `draft-v0.2.md` - 第一轮反馈后
- `draft-v0.3.md` - 第二轮反馈后
- 等

这允许跟踪博客文章的演变并在需要时进行恢复。

## 输出文件结构
```
blog-posts/
└── YYYY-MM-DD-topic-name/
    ├── resources/
    │   ├── source-1-name.md
    │   ├── source-2-name.md
    │   └── ...
    ├── OUTLINE.md
    ├── draft-v0.1.md
    ├── draft-v0.2.md (if iterations)
    └── draft-v0.3.md (if more iterations)
```
## 质量提示

- **hooks**：从问题、令人惊讶的事实或相关场景开始
- **流程**：每个段落应连接到下一个段落
- **证据**：用研究数据支持主张
- **引文**：始终引用以下内容的来源：
  - 所有统计数据和数据点（例如，“根据[来源]，75%...”）
- 产品、服务或方法之间的比较（例如，“X 的执行速度比 Y 快 2 倍[来源]”）
  - 有关市场趋势、研究结果或基准的事实主张
  - 使用内联引用，格式为：[来源名称]或[作者，年份]
- **声音**：始终保持一致的语气
- **长度**：尊重目标字数
- **可读性**：在适当的情况下使用短段落、要点
- **CTA**：以明确的号召性用语或发人深省的问题结束

## 注释

- 始终在概述的检查点等待用户批准
- 保留所有草稿版本以供历史记录
- 提供 URL 后，使用网络搜索获取最新信息
- 如果资源不足，请向用户询问更多信息或建议进行额外的研究
- 根据目标受众（技术、一般、业务等）调整语气
