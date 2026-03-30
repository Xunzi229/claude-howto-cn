---
姓名：claude-MD
描述：按照最佳 AI agents入门最佳实践创建或更新 CLAUDE.md 文件
---

## 用户输入
```text
$ARGUMENTS
```
在继续之前，您**必须**考虑用户输入（如果不为空）。用户可以指定：
- `create` - 从头开始创建新的 CLAUDE.md
- `update` - 改进现有的 CLAUDE.md
- `audit` - 分析并报告当前 CLAUDE.md 质量
- 创建/更新的特定路径（例如，`src/api/CLAUDE.md` 用于特定于目录的指令）

## 核心原则

**LLM 是无状态的**：CLAUDE.md 是唯一一个自动包含在每个对话中的文件。它是人工智能agents进入代码库的主要入门文档。

### 黄金法则

1. **少即是多**：前沿LLM可以遵循约 150-200 条指令。Claude Code's的系统提示符已经使用了~50。让您的 CLAUDE.md 保持重点和简洁。

2. **普遍适用性**：仅包含与每个会话相关的信息。特定于任务的指令位于单独的文件中。

3. **不要使用 Claude 作为 Linter**：风格指南会使上下文变得臃肿并降低指令遵循性。请改用确定性工具（prettier、eslint 等）。

4. **从不自动生成**：CLAUDE.md 是 AI 工具的最高杠杆点。经过仔细考虑后手动制作。

## 执行流程

### 1. 项目分析

首先分析一下当前项目状态：

1. 检查现有的 CLAUDE.md 文件：
- 根级别：`./CLAUDE.md` 或 `.claude/CLAUDE.md`
- 目录特定：`**/CLAUDE.md`
- 全局用户配置：`~/.claude/CLAUDE.md`

2.确定项目结构：
   - 技术栈（语言、框架）
   - 项目类型（单一应用程序、单一应用程序、库）
   - 开发工具（包管理器、构建系统、测试运行器）

3.审查现有文档：
- 自述文件.md
- 贡献.md
- package.json、pyproject.toml、Cargo.toml 等

### 2. 内容策略（什么、为什么、如何）

围绕三个维度构建 CLAUDE.md：

#### 什么 - 技术与结构
- 技术栈概述
- 项目组织（对于 monorepos 尤其重要）
- 关键目录及其用途

#### 为什么 - 目的和背景
- 该项目的作用
- 为什么做出某些架构决策
- 每个主要组件负责什么

#### HOW - 工作流程和约定
- 开发工作流程（bun vs node、pip vs uv 等）
- 测试程序和命令
- 验证和构建方法
- 关键的“陷阱”或不明显的要求

### 3. 渐进式披露策略

对于较大的项目，建议创建一个 `agent_docs/` 文件夹：
```
agent_docs/
  |- building_the_project.md
  |- running_tests.md
  |- code_conventions.md
  |- architecture_decisions.md
```
在 CLAUDE.md 中，使用如下指令引用这些文件：
```markdown
For detailed build instructions, refer to `agent_docs/building_the_project.md`
```
**重要**：使用 `file:line` 引用而不是代码片段以避免过时的上下文。

### 4. 质量限制

创建或更新 CLAUDE.md 时：

1. **目标长度**：300 行以下（理想情况下 100 行以下）
2. **无样式规则**：删除任何 linting/格式化说明
3. **无特定于任务的说明**：移至单独的文件
4. **无代码片段**：使用文件引用代替
5. **无冗余信息**：不要重复 package.json 或 README 中的内容

### 5. 基本部分

结构良好的 CLAUDE.md 应包括：
```markdown
# Project Name

Brief one-line description.

## Tech Stack
- Primary language and version
- Key frameworks/libraries
- Database/storage (if any)

## Project Structure
[Only for monorepos or complex structures]
- `apps/` - Application entry points
- `packages/` - Shared libraries

## Development Commands
- Install: `command`
- Test: `command`
- Build: `command`

## Critical Conventions
[Only non-obvious, high-impact conventions]
- Convention 1 with brief explanation
- Convention 2 with brief explanation

## Known Issues / Gotchas
[Things that consistently trip up developers]
- Issue 1
- Issue 2
```
### 6. 要避免的反模式

**不包括：**
- 代码风格指南（使用 linter）
- 有关如何使用claude的文档
- 对明显模式的详细解释
- 复制粘贴代码示例
- 通用最佳实践（“编写干净的代码”）
- 具体任务的说明
- 自动生成的内容
- 广泛的待办事项列表

### 7. 验证清单

在最终确定之前，请验证：

- [ ] 300行以下（最好100行以下）
- [ ] 每行适用于所有会话
- [ ] 无样式/格式规则
- [ ] 无代码片段（使用文件引用）
- [ ] 命令已验证有效
- [ ] 用于复杂项目的渐进式披露
- [ ] 记录了关键问题
- [ ] 与 README.md 没有冗余

## 输出格式

### 对于 `create` 或默认值：

1. 分析项目
2. 按照上述结构起草一个 CLAUDE.md
3、送审稿
4. 批准后写信至适当地点

### 对于 `update`：

1.阅读已有的CLAUDE.md
2. 根据最佳实践进行审核
3. 识别：
   - 要删除的内容（样式规则、代码片段、特定于任务）
   - 内容要浓缩
   - 缺少重要信息
4. 提交变更以供审核
5. 批准后应用变更

### 对于 `audit`：

1.阅读已有的CLAUDE.md
2. 生成报告：
   - 当前行数与目标行数
   - 普遍适用内容的百分比
   - 发现的反模式列表
   - 改进建议
3. 不要修改文件，仅报告

## AGENTS.md 处理

如果用户请求 AGENTS.md 创建/更新：

AGENTS.md 用于定义专门的agents行为。与 CLAUDE.md（用于项目上下文）不同，AGENTS.md 定义：
- 自定义agents角色和功能
- agents特定的指令和限制
- 多agents场景的工作流程定义

应用类似的原则：
- 保持重点和简洁
- 使用渐进式披露
- 参考外部文档而不是嵌入内容

## 注释

- 在包含命令之前始终验证命令是否有效
- 如有疑问，请忽略 - 少即是多
- 系统提醒告诉claude，CLAUDE.md“可能相关，也可能不相关”——噪音越多，就越容易被忽略
- Monorepos 从清晰的“WHAT/WHY/HOW”结构中获益最多
- 特定于目录的 CLAUDE.md 文件应该更加集中
