<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# claude概念完整指南

全面的参考指南，涵盖 Slash 命令、Subagents、内存、MCP 协议和agents、skills，包含表格、图表和实际示例。

---

## 目录

1.[Slash Commands](#slash-commands)
2.[Subagents](#subagents)
3.[Memory](#memory)
4.[MCP Protocol](#mcp-protocol)
5.[Agent Skills](#agent-skills)
6.[Plugins](#plugins)
7.[Hooks](#hooks)
8.[Checkpoints and Rewind](#checkpoints-and-rewind)
9.[Advanced Features](#advanced-features)
10.[Comparison & Integration](#comparison--integration)

---

## 斜线命令

### 概述

斜杠命令是用户调用的快捷方式，存储为 Claude Code 可以执行的 Markdown 文件。它们使团队能够标准化常用的提示和工作流程。

＃＃＃ 建筑学
```mermaid
graph TD
    A["User Input: /command-name"] -->|Triggers| B["Search .claude/commands/"]
    B -->|Finds| C["command-name.md"]
    C -->|Loads| D["Markdown Content"]
    D -->|Executes| E["Claude Processes Prompt"]
    E -->|Returns| F["Result in Context"]
```
### 文件结构
```mermaid
graph LR
    A["Project Root"] -->|contains| B[".claude/commands/"]
    B -->|contains| C["optimize.md"]
    B -->|contains| D["test.md"]
    B -->|contains| E["docs/"]
    E -->|contains| F["generate-api-docs.md"]
    E -->|contains| G["generate-readme.md"]
```
### 命令组织表

|地点 |范围 |可用性 |使用案例| Git 跟踪 |
|----------|---------|--------------|----------|------------|
| `.claude/commands/` |项目特定|团队成员 |团队工作流程、共享标准 | ✅ 是的 |
| `~/.claude/commands/` |个人|个人用户|跨项目的个人快捷方式| ❌ 否 |
|子目录|命名空间|基于父|按类别整理 | ✅ 是的 |

### 特性和功能

|特色|示例|支持 |
|--------|---------|------------|
| Shell 脚本执行 | `bash scripts/deploy.sh` | ✅ 是的 |
|文件参考| `@path/to/file.js` | ✅ 是的 |
| Bash 集成 | `$(git log --oneline)` | ✅ 是的 |
|论点| `/pr --verbose` | ✅ 是的 |
| MCP 命令 | `/mcp__github__list_prs` | ✅ 是的 |

### 实际例子

#### 示例1：代码优化命令

**文件：** `.claude/commands/optimize.md`
```markdown
---
name: Code Optimization
description: Analyze code for performance issues and suggest optimizations
tags: performance, analysis
---

# Code Optimization

Review the provided code for the following issues in order of priority:

1. **Performance bottlenecks** - identify O(n²) operations, inefficient loops
2. **Memory leaks** - find unreleased resources, circular references
3. **Algorithm improvements** - suggest better algorithms or data structures
4. **Caching opportunities** - identify repeated computations
5. **Concurrency issues** - find race conditions or threading problems

Format your response with:
- Issue severity (Critical/High/Medium/Low)
- Location in code
- Explanation
- Recommended fix with code example
```
**用法：**
```bash
# User types in Claude Code
/optimize

# Claude loads the prompt and waits for code input
```
#### 示例 2：拉取请求帮助程序命令

**文件：** `.claude/commands/pr.md`
```markdown
---
name: Prepare Pull Request
description: Clean up code, stage changes, and prepare a pull request
tags: git, workflow
---

# Pull Request Preparation Checklist

Before creating a PR, execute these steps:

1. Run linting: `prettier --write .`
2. Run tests: `npm test`
3. Review git diff: `git diff HEAD`
4. Stage changes: `git add .`
5. Create commit message following conventional commits:
   - `fix:` for bug fixes
   - `feat:` for new features
   - `docs:` for documentation
   - `refactor:` for code restructuring
   - `test:` for test additions
   - `chore:` for maintenance

6. Generate PR summary including:
   - What changed
   - Why it changed
   - Testing performed
   - Potential impacts
```
**用法：**
```bash
/pr

# Claude runs through checklist and prepares the PR
```
#### 示例 3：分层文档生成器

**文件：** `.claude/commands/docs/generate-api-docs.md`
```markdown
---
name: Generate API Documentation
description: Create comprehensive API documentation from source code
tags: documentation, api
---

# API Documentation Generator

Generate API documentation by:

1. Scanning all files in `/src/api/`
2. Extracting function signatures and JSDoc comments
3. Organizing by endpoint/module
4. Creating markdown with examples
5. Including request/response schemas
6. Adding error documentation

Output format:
- Markdown file in `/docs/api.md`
- Include curl examples for all endpoints
- Add TypeScript types
```
### 命令生命周期图
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: Types /optimize
    Claude->>FS: Searches .claude/commands/
    FS-->>Claude: Returns optimize.md
    Claude->>Claude: Loads Markdown content
    Claude->>User: Displays prompt context
    User->>Claude: Provides code to analyze
    Claude->>CLI: (May execute scripts)
    CLI-->>Claude: Results
    Claude->>User: Returns analysis
```
### 最佳实践

| ✅ 做 | ❌不要|
|------|---------|
|使用清晰、面向行动的名称 |为一次性任务创建命令 |
|文档描述中的触发词 |在命令中构建复杂的逻辑 |
|让命令集中于单一任务 |创建冗余命令 |
|版本控制项目命令 |硬编码敏感信息 |
|在子目录中组织 |创建长命令列表 |
|使用简单易读的提示 |使用缩写或隐晦的措辞 |

---

## Subagents

### 概述

Subagents是专门的人工智能助手，具有独立的上下文窗口和定制的系统提示。它们支持委派任务执行，同时保持关注点的清晰分离。

### 架构图
```mermaid
graph TB
    User["👤 User"]
    Main["🎯 Main Agent<br/>(Coordinator)"]
    Reviewer["🔍 Code Reviewer<br/>Subagent"]
    Tester["✅ Test Engineer<br/>Subagent"]
    Docs["📝 Documentation<br/>Subagent"]

    User -->|asks| Main
    Main -->|delegates| Reviewer
    Main -->|delegates| Tester
    Main -->|delegates| Docs
    Reviewer -->|returns result| Main
    Tester -->|returns result| Main
    Docs -->|returns result| Main
    Main -->|synthesizes| User
```
### Subagents生命周期
```mermaid
sequenceDiagram
    participant User
    participant MainAgent as Main Agent
    participant CodeReviewer as Code Reviewer<br/>Subagent
    participant Context as Separate<br/>Context Window

    User->>MainAgent: "Build new auth feature"
    MainAgent->>MainAgent: Analyze task
    MainAgent->>CodeReviewer: "Review this code"
    CodeReviewer->>Context: Initialize clean context
    Context->>CodeReviewer: Load reviewer instructions
    CodeReviewer->>CodeReviewer: Perform review
    CodeReviewer-->>MainAgent: Return findings
    MainAgent->>MainAgent: Incorporate results
    MainAgent-->>User: Provide synthesis
```
### Subagents配置表

|配置|类型 |目的|示例|
|----------------|------|---------|---------|
| `name` |字符串|agents标识符| `code-reviewer` |
| `description` |字符串|目的和触发条件 | `Comprehensive code quality analysis` |
| `tools` |列表/字符串|允许的功能 | `read, grep, diff, lint_runner` |
| `system_prompt` |降价|行为指示|定制指南|

### 工具访问层次结构
```mermaid
graph TD
    A["Subagent Configuration"] -->|Option 1| B["Inherit All Tools<br/>from Main Thread"]
    A -->|Option 2| C["Specify Individual Tools"]
    B -->|Includes| B1["File Operations"]
    B -->|Includes| B2["Shell Commands"]
    B -->|Includes| B3["MCP Tools"]
    C -->|Explicit List| C1["read, grep, diff"]
    C -->|Explicit List| C2["Bash(npm:*), Bash(test:*)"]
```
### 实际例子

#### 示例 1：完整的Subagents设置

**文件：** `.claude/agents/code-reviewer.md`
```yaml
---
name: code-reviewer
description: Comprehensive code quality and maintainability analysis
tools: read, grep, diff, lint_runner
---

# Code Reviewer Agent

You are an expert code reviewer specializing in:
- Performance optimization
- Security vulnerabilities
- Code maintainability
- Testing coverage
- Design patterns

## Review Priorities (in order)

1. **Security Issues** - Authentication, authorization, data exposure
2. **Performance Problems** - O(n²) operations, memory leaks, inefficient queries
3. **Code Quality** - Readability, naming, documentation
4. **Test Coverage** - Missing tests, edge cases
5. **Design Patterns** - SOLID principles, architecture

## Review Output Format

For each issue:
- **Severity**: Critical / High / Medium / Low
- **Category**: Security / Performance / Quality / Testing / Design
- **Location**: File path and line number
- **Issue Description**: What's wrong and why
- **Suggested Fix**: Code example
- **Impact**: How this affects the system

## Example Review

### Issue: N+1 Query Problem
- **Severity**: High
- **Category**: Performance
- **Location**: src/user-service.ts:45
- **Issue**: Loop executes database query in each iteration
- **Fix**: Use JOIN or batch query
```
**文件：** `.claude/agents/test-engineer.md`
```yaml
---
name: test-engineer
description: Test strategy, coverage analysis, and automated testing
tools: read, write, bash, grep
---

# Test Engineer Agent

You are expert at:
- Writing comprehensive test suites
- Ensuring high code coverage (>80%)
- Testing edge cases and error scenarios
- Performance benchmarking
- Integration testing

## Testing Strategy

1. **Unit Tests** - Individual functions/methods
2. **Integration Tests** - Component interactions
3. **End-to-End Tests** - Complete workflows
4. **Edge Cases** - Boundary conditions
5. **Error Scenarios** - Failure handling

## Test Output Requirements

- Use Jest for JavaScript/TypeScript
- Include setup/teardown for each test
- Mock external dependencies
- Document test purpose
- Include performance assertions when relevant

## Coverage Requirements

- Minimum 80% code coverage
- 100% for critical paths
- Report missing coverage areas
```
**文件：** `.claude/agents/documentation-writer.md`
```yaml
---
name: documentation-writer
description: Technical documentation, API docs, and user guides
tools: read, write, grep
---

# Documentation Writer Agent

You create:
- API documentation with examples
- User guides and tutorials
- Architecture documentation
- Changelog entries
- Code comment improvements

## Documentation Standards

1. **Clarity** - Use simple, clear language
2. **Examples** - Include practical code examples
3. **Completeness** - Cover all parameters and returns
4. **Structure** - Use consistent formatting
5. **Accuracy** - Verify against actual code

## Documentation Sections

### For APIs
- Description
- Parameters (with types)
- Returns (with types)
- Throws (possible errors)
- Examples (curl, JavaScript, Python)
- Related endpoints

### For Features
- Overview
- Prerequisites
- Step-by-step instructions
- Expected outcomes
- Troubleshooting
- Related topics
```
#### 示例 2：实际的Subagents委派
```markdown
# Scenario: Building a Payment Feature

## User Request
"Build a secure payment processing feature that integrates with Stripe"

## Main Agent Flow

1. **Planning Phase**
   - Understands requirements
   - Determines tasks needed
   - Plans architecture

2. **Delegates to Code Reviewer Subagent**
   - Task: "Review the payment processing implementation for security"
   - Context: Auth, API keys, token handling
   - Reviews for: SQL injection, key exposure, HTTPS enforcement

3. **Delegates to Test Engineer Subagent**
   - Task: "Create comprehensive tests for payment flows"
   - Context: Success scenarios, failures, edge cases
   - Creates tests for: Valid payments, declined cards, network failures, webhooks

4. **Delegates to Documentation Writer Subagent**
   - Task: "Document the payment API endpoints"
   - Context: Request/response schemas
   - Produces: API docs with curl examples, error codes

5. **Synthesis**
   - Main agent collects all outputs
   - Integrates findings
   - Returns complete solution to user
```
#### 示例 3：工具权限范围

**限制性设置 - 仅限于特定命令**
```yaml
---
name: secure-reviewer
description: Security-focused code review with minimal permissions
tools: read, grep
---

# Secure Code Reviewer

Reviews code for security vulnerabilities only.

This agent:
- ✅ Reads files to analyze
- ✅ Searches for patterns
- ❌ Cannot execute code
- ❌ Cannot modify files
- ❌ Cannot run tests

This ensures the reviewer doesn't accidentally break anything.
```
**扩展设置 - 所有实施工具**
```yaml
---
name: implementation-agent
description: Full implementation capabilities for feature development
tools: read, write, bash, grep, edit, glob
---

# Implementation Agent

Builds features from specifications.

This agent:
- ✅ Reads specifications
- ✅ Writes new code files
- ✅ Runs build commands
- ✅ Searches codebase
- ✅ Edits existing files
- ✅ Finds files matching patterns

Full capabilities for independent feature development.
```
### Subagents上下文管理
```mermaid
graph TB
    A["Main Agent Context<br/>50,000 tokens"]
    B["Subagent 1 Context<br/>20,000 tokens"]
    C["Subagent 2 Context<br/>20,000 tokens"]
    D["Subagent 3 Context<br/>20,000 tokens"]

    A -->|Clean slate| B
    A -->|Clean slate| C
    A -->|Clean slate| D

    B -->|Results only| A
    C -->|Results only| A
    D -->|Results only| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```
### 何时使用Subagents

|场景 |使用Subagents |为什么 |
|----------|--------------|-----|
|具有多个步骤的复杂功能 | ✅ 是的 |分离关注点，防止上下文污染 |
|快速代码审查 | ❌ 否 |没有必要的开销|
|并行任务执行| ✅ 是的 |每个Subagents都有自己的上下文 |
|需要专业知识 | ✅ 是的 |自定义系统提示|
|长期运行分析 | ✅ 是的 |防止主上下文耗尽 |
|单任务 | ❌ 否 |不必要地增加延迟 |

### 特工团队

agents团队协调多个agents执行相关任务。agents团队不是一次委托给一个Subagents，而是允许主agents协调一组agents，这些agents进行协作、共享中间结果并朝着共同目标努力。这对于前端agents、后端agents和测试agents并行工作的全栈功能开发等大规模任务非常有用。

---

## 内存

### 概述

记忆使claude能够保留会话和对话中的上下文。它以两种形式存在：claude.ai 中的自动合成，以及 Claude Code 中基于文件系统的 CLAUDE.md。

### 内存架构
```mermaid
graph TB
    A["Claude Session"]
    B["User Input"]
    C["Memory System"]
    D["Memory Storage"]

    B -->|User provides info| C
    C -->|Synthesizes every 24h| D
    D -->|Loads automatically| A
    A -->|Uses context| C
```
### Claude 代码中的内存层次结构（7 层）

Claude Code 从 7 层加载内存，从最高优先级到最低优先级列出：
```mermaid
graph TD
    A["1. Managed Policy<br/>Enterprise admin policies"] --> B["2. Project Memory<br/>./CLAUDE.md"]
    B --> C["3. Project Rules<br/>.claude/rules/*.md"]
    C --> D["4. User Memory<br/>~/.claude/CLAUDE.md"]
    D --> E["5. User Rules<br/>~/.claude/rules/*.md"]
    E --> F["6. Local Memory<br/>.claude/local/CLAUDE.md"]
    F --> G["7. Auto Memory<br/>Automatically captured preferences"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```
### 内存位置表

|等级 |地点 |范围 |优先|共享|最适合 |
|------|----------|--------|----------|--------|----------|
| 1. 管理策略|企业管理|组织|最高|所有组织用户 |合规性、安全政策|
| 2. 项目 | `./CLAUDE.md` |项目|高|团队（Git）|团队标准、架构|
| 3.项目规则| `.claude/rules/*.md` |项目|高|团队（Git）|模块化项目约定|
| 4. 用户 | `~/.claude/CLAUDE.md` |个人|中等|个人|个人喜好|
| 5. 用户规则| `~/.claude/rules/*.md` |个人|中等|个人|个人规则模块 |
| 6.本地| `.claude/local/CLAUDE.md` |本地|低|未共享 |机器特定设置|
| 7.自动记忆|自动|会议|最低|个人|了解偏好、模式 |

### 自动记忆

自动记忆会自动捕获会话期间观察到的用户偏好和模式。claude从你们的互动中学习并记住：

- 编码风格偏好
- 您所做的常见更正
- 框架和工具选择
- 沟通方式偏好

自动记忆在后台工作，不需要手动配置。

### 内存更新生命周期
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as File System
    participant Memory as CLAUDE.md

    User->>Claude: "Remember: use async/await"
    Claude->>User: "Which memory file?"
    User->>Claude: "Project memory"
    Claude->>Editor: Open ~/.claude/settings.json
    Claude->>Memory: Write to ./CLAUDE.md
    Memory-->>Claude: File saved
    Claude->>Claude: Load updated memory
    Claude-->>User: "Memory saved!"
```
### 实际例子

#### 示例 1：项目内存结构

**文件：** `./CLAUDE.md`
```markdown
# Project Configuration

## Project Overview
- **Name**: E-commerce Platform
- **Tech Stack**: Node.js, PostgreSQL, React 18, Docker
- **Team Size**: 5 developers
- **Deadline**: Q4 2025

## Architecture
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## Development Standards

### Code Style
- Use Prettier for formatting
- Use ESLint with airbnb config
- Maximum line length: 100 characters
- Use 2-space indentation

### Naming Conventions
- **Files**: kebab-case (user-controller.js)
- **Classes**: PascalCase (UserService)
- **Functions/Variables**: camelCase (getUserById)
- **Constants**: UPPER_SNAKE_CASE (API_BASE_URL)
- **Database Tables**: snake_case (user_accounts)

### Git Workflow
- Branch names: `feature/description` or `fix/description`
- Commit messages: Follow conventional commits
- PR required before merge
- All CI/CD checks must pass
- Minimum 1 approval required

### Testing Requirements
- Minimum 80% code coverage
- All critical paths must have tests
- Use Jest for unit tests
- Use Cypress for E2E tests
- Test filenames: `*.test.ts` or `*.spec.ts`

### API Standards
- RESTful endpoints only
- JSON request/response
- Use HTTP status codes correctly
- Version API endpoints: `/api/v1/`
- Document all endpoints with examples

### Database
- Use migrations for schema changes
- Never hardcode credentials
- Use connection pooling
- Enable query logging in development
- Regular backups required

### Deployment
- Docker-based deployment
- Kubernetes orchestration
- Blue-green deployment strategy
- Automatic rollback on failure
- Database migrations run before deploy

## Common Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start development server |
| `npm test` | Run test suite |
| `npm run lint` | Check code style |
| `npm run build` | Build for production |
| `npm run migrate` | Run database migrations |

## Team Contacts
- Tech Lead: Sarah Chen (@sarah.chen)
- Product Manager: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## Known Issues & Workarounds
- PostgreSQL connection pooling limited to 20 during peak hours
- Workaround: Implement query queuing
- Safari 14 compatibility issues with async generators
- Workaround: Use Babel transpiler

## Related Projects
- Analytics Dashboard: `/projects/analytics`
- Mobile App: `/projects/mobile`
- Admin Panel: `/projects/admin`
```
#### 示例 2：特定于目录的内存

**文件：** `./src/api/CLAUDE.md`

~~~~降价
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
### 错误响应：
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
～～～

#### 示例 3：个人记忆

**文件：** `~/.claude/CLAUDE.md`

~~~~降价
# 我的开发偏好

## 关于我
- **经验水平**：8 年全栈开发经验
- **首选语言**：TypeScript、Python
- **沟通方式**：直接，带有示例
- **学习风格**：带有代码的可视化图表

## 代码首选项

### 错误处理
我更喜欢使用 try-catch 块和有意义的错误消息进行显式错误处理。
避免一般性错误。始终记录错误以进行调试。

### 评论
使用注释来说明“为什么”，而不是“什么”。代码应该是自记录的。
注释应该解释业务逻辑或非显而易见的决策。

### 测试
我更喜欢TDD（测试驱动开发）。
先编写测试，然后实施。
关注行为，而不是实施细节。

### 架构
我更喜欢模块化、松耦合的设计。
使用依赖注入来实现可测试性。
单独关注（控制器、服务、存储库）。

## 调试首选项
- 使用带有前缀的console.log：`[DEBUG]`
- 包括上下文：函数名称、相关变量
- 可用时使用堆栈跟踪
- 始终在日志中包含时间戳

## 通讯
- 用图表解释复杂的概念
- 在解释理论之前展示具体例子
- 包括之前/之后的代码片段
- 最后总结要点

## 项目组织
我将我的项目组织为：
```
project/
  ├── src/
  │   ├── api/
  │   ├── services/
  │   ├── models/
  │   └── utils/
  ├── tests/
  ├── docs/
  └── docker/
```
## 工具
- **IDE**：带有 vim 键绑定的 VS Code
- **终端**：带有 Oh-My-Zsh 的 Zsh
- **格式**：更漂亮（100 字符行长度）
- **Linter**：带有airbnb配置的ESLint
- **测试框架**：带有 React 测试库的 Jest
～～～

#### 示例 4：会话期间内存更新

**会话互动：**
```markdown
User: Remember that I prefer using React hooks instead of class components
     for all new components.

Claude: I'm adding that to your memory. Which memory file should this go in?
        1. Project memory (./CLAUDE.md)
        2. Personal memory (~/.claude/CLAUDE.md)

User: Project memory

Claude: ✅ Memory saved!

Added to ./CLAUDE.md:
---

### Component Development
- Use functional components with React Hooks
- Prefer hooks over class components
- Custom hooks for reusable logic
- Use useCallback for event handlers
- Use useMemo for expensive computations
```
### Claude Web/桌面中的内存

#### 内存合成时间线
```mermaid
graph LR
    A["Day 1: User<br/>Conversations"] -->|24 hours| B["Day 2: Memory<br/>Synthesis"]
    B -->|Automatic| C["Memory Updated<br/>Summarized"]
    C -->|Loaded in| D["Day 2-N:<br/>New Conversations"]
    D -->|Add to| E["Memory"]
    E -->|24 hours later| F["Memory Refreshed"]
```
**内存摘要示例：**
```markdown
## Claude's Memory of User

### Professional Background
- Senior full-stack developer with 8 years experience
- Focus on TypeScript/Node.js backends and React frontends
- Active open source contributor
- Interested in AI and machine learning

### Project Context
- Currently building e-commerce platform
- Tech stack: Node.js, PostgreSQL, React 18, Docker
- Working with team of 5 developers
- Using CI/CD and blue-green deployments

### Communication Preferences
- Prefers direct, concise explanations
- Likes visual diagrams and examples
- Appreciates code snippets
- Explains business logic in comments

### Current Goals
- Improve API performance
- Increase test coverage to 90%
- Implement caching strategy
- Document architecture
```
### 内存特性比较

|特色|claude网络/桌面|claude代码 (CLAUDE.md) |
|--------|--------------------|------------------------|
|自动合成| ✅ 每 24 小时 | ❌ 手册 |
|跨项目| ✅ 共享 | ❌ 项目特定 |
|团队访问| ✅ 共享项目 | ✅ Git 跟踪 |
|可搜索| ✅ 内置 | ✅ 通过 `/memory` |
|可编辑| ✅ 聊天中 | ✅ 直接文件编辑 |
|进出口| ✅ 是的 | ✅ 复制/粘贴 |
|坚持不懈| ✅ 24 小时以上 | ✅ 无限期 |

---

## MCP 协议

### 概述

MCP（模型上下文协议）是 Claude 访问外部工具、API 和实时数据源的标准化方式。与内存不同，MCP 提供对不断变化的数据的实时访问。

### MCP 架构
```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["External Service"]

    A -->|Request: list_issues| B
    B -->|Query| C
    C -->|Data| B
    B -->|Response| A

    A -->|Request: create_issue| B
    B -->|Action| C
    C -->|Result| B
    B -->|Response| A
```
### MCP 生态系统
```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|File I/O| G["Local Files"]
    C -->|API| H["GitHub Repos"]
    D -->|Query| I["PostgreSQL/MySQL"]
    E -->|Messages| J["Slack Workspace"]
    F -->|Docs| K["Google Drive"]
```
### MCP 设置过程
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: Type /mcp
    Claude->>Claude: List available MCP servers
    Claude->>User: Show options
    User->>Claude: Select GitHub MCP
    Claude->>Config: Update configuration
    Config->>Claude: Activate connection
    Claude->>Service: Test connection
    Service-->>Claude: Authentication successful
    Claude->>User: ✅ MCP connected!
```
### 可用 MCP 服务器表

| MCP 服务器 |目的|常用工具|授权 |实时|
|------------|---------|--------------|-----|------------|
| **文件系统** |文件操作 |读、写、删除|操作系统权限 | ✅ 是的 |
| **GitHub** |存储库管理| list_prs、create_issue、推送 | OAuth | ✅ 是的 |
| **Slack** |团队沟通|发送消息、列表频道 |tokens| ✅ 是的 |
| **数据库** | SQL 查询 |查询、插入、更新 |证书 | ✅ 是的 |
| **Google Docs** |文档访问 |阅读、写作、分享 | OAuth | ✅ 是的 |
| **Asana** |项目管理|创建任务、更新状态 | API 密钥 | ✅ 是的 |
| **Stripe** |付款数据|列表费用，创建发票 | API 密钥 | ✅ 是的 |
| **内存** |持久记忆|存储、检索、删除 |本地| ❌ 否 |

### 实际例子

#### 示例 1：GitHub MCP 配置

**文件：** `.mcp.json`（项目范围）或 `~/.claude.json`（用户范围）
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```
**可用的 GitHub MCP 工具：**

~~~~降价
# GitHub MCP 工具

## 拉取请求管理
- `list_prs` - 列出存储库中的所有 PR
- `get_pr` - 获取 PR 详细信息，包括差异
- `create_pr` - 创建新 PR
- `update_pr` - 更新公关描述/标题
- `merge_pr` - 将 PR 合并到主分支
- `review_pr` - 添加评论意见

请求示例：
```
/mcp__github__get_pr 456

# Returns:
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```
## 问题管理
- `list_issues` - 列出所有问题
- `get_issue` - 获取问题详细信息
- `create_issue` - 创建新问题
- `close_issue` - 关闭问题
- `add_comment` - 添加评论到问题

## 存储库信息
- `get_repo_info` - 存储库详细信息
- `list_files` - 文件树结构
- `get_file_content` - 读取文件内容
- `search_code` - 跨代码库搜索

## 提交操作
- `list_commits` - 提交历史记录
- `get_commit` - 具体提交详细信息
- `create_commit` - 创建新提交
～～～

#### 示例 2：数据库 MCP 设置

**配置：**
```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```
**用法示例：**
```markdown
User: Fetch all users with more than 10 orders

Claude: I'll query your database to find that information.

# Using MCP database tool:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# Results:
- Alice: 15 orders
- Bob: 12 orders
- Charlie: 11 orders
```
#### 示例 3：多 MCP 工作流程

**场景：生成日报**
```markdown
# Daily Report Workflow using Multiple MCPs

## Setup
1. GitHub MCP - fetch PR metrics
2. Database MCP - query sales data
3. Slack MCP - post report
4. Filesystem MCP - save report

## Workflow

### Step 1: Fetch GitHub Data
/mcp__github__list_prs completed:true last:7days

Output:
- Total PRs: 42
- Average merge time: 2.3 hours
- Review turnaround: 1.1 hours

### Step 2: Query Database
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

Output:
- Sales: 247
- Revenue: $12,450

### Step 3: Generate Report
Combine data into HTML report

### Step 4: Save to Filesystem
Write report.html to /reports/

### Step 5: Post to Slack
Send summary to #daily-reports channel

Final Output:
✅ Report generated and posted
📊 47 PRs merged this week
💰 $12,450 in daily sales
```
#### 示例 4：文件系统 MCP 操作

**配置：**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```
**可用操作：**

|运营|命令 |目的|
|------------|---------|---------|
|列出文件 | `ls ~/projects` |显示目录内容 |
|读取文件 | `cat src/main.ts` |读取文件内容 |
|写入文件| `create docs/api.md` |创建新文件 |
|编辑文件 | `edit src/app.ts` |修改文件|
|搜索 | `grep "async function"` |在文件中搜索 |
|删除 | `rm old-file.js` |删除文件 |

### MCP 与内存：决策矩阵
```mermaid
graph TD
    A["Need external data?"]
    A -->|No| B["Use Memory"]
    A -->|Yes| C["Does it change frequently?"]
    C -->|No/Rarely| B
    C -->|Yes/Often| D["Use MCP"]

    B -->|Stores| E["Preferences<br/>Context<br/>History"]
    D -->|Accesses| F["Live APIs<br/>Databases<br/>Services"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```
### 请求/响应模式
```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as Database

    App->>MCP: Request: "SELECT * FROM users WHERE id=1"
    MCP->>DB: Execute query
    DB-->>MCP: Result set
    MCP-->>App: Return parsed data
    App->>App: Process result
    App->>App: Continue task

    Note over MCP,DB: Real-time access<br/>No caching
```
---

## agents、skills

### 概述

agents、skills是可重用的模型调用功能，打包为包含指令、脚本和资源的文件夹。claude自动检测并使用相关skills。

### skills架构
```mermaid
graph TB
    A["Skill Directory"]
    B["SKILL.md"]
    C["YAML Metadata"]
    D["Instructions"]
    E["Scripts"]
    F["Templates"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```
### skills加载过程
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as System
    participant Skill as Skill

    User->>Claude: "Create Excel report"
    Claude->>System: Scan available skills
    System->>System: Load skill metadata
    Claude->>Claude: Match user request to skills
    Claude->>Skill: Load xlsx skill SKILL.md
    Skill-->>Claude: Return instructions + tools
    Claude->>Claude: Execute skill
    Claude->>User: Generate Excel file
```
### skills类型和位置表

|类型 |地点 |范围 |共享|同步 |最适合 |
|------|----------|--------|--------|-----|----------|
|预建|内置|全球|所有用户|汽车 |文档创建|
|个人| `~/.claude/skills/` |个人|没有 |手册|个人自动化|
|项目| `.claude/skills/` |团队|是的 | git | git团队标准|
|Plugins |通过Plugins安装 |变化 |取决于 |汽车 |集成功能|

### 预建skills
```mermaid
graph TB
    A["Pre-built Skills"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["Create presentations"]
    B --> B2["Edit slides"]
    C --> C1["Create spreadsheets"]
    C --> C2["Analyze data"]
    D --> D1["Create documents"]
    D --> D2["Format text"]
    E --> E1["Generate PDFs"]
    E --> E2["Fill forms"]
```
### 捆绑skills

Claude Code 现在包含 5 种开箱即用的捆绑skills：

|skills|命令 |目的|
|--------|---------|---------|
| **简化** | `/simplify` |简化复杂的代码或解释 |
| **批次** | `/batch` |跨多个文件或项目运行操作 |
| **调试** | `/debug` |系统调试问题并分析根本原因|
| **循环** | `/loop` |在计时器上安排重复任务 |
| **claude·API** | `/claude-api` |直接与 Anthropic API 交互 |

这些捆绑skills始终可用，无需安装或配置。

### 实际例子

#### 示例 1：自定义代码审查技巧

**目录结构：**
```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```
**文件：** `~/.claude/skills/code-review/SKILL.md`
```yaml
---
name: Code Review Specialist
description: Comprehensive code review with security, performance, and quality analysis
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: When users ask to review code, analyze code quality, or evaluate pull requests
effort: high
shell: bash
---

# Code Review Skill

This skill provides comprehensive code review capabilities focusing on:

1. **Security Analysis**
   - Authentication/authorization issues
   - Data exposure risks
   - Injection vulnerabilities
   - Cryptographic weaknesses
   - Sensitive data logging

2. **Performance Review**
   - Algorithm efficiency (Big O analysis)
   - Memory optimization
   - Database query optimization
   - Caching opportunities
   - Concurrency issues

3. **Code Quality**
   - SOLID principles
   - Design patterns
   - Naming conventions
   - Documentation
   - Test coverage

4. **Maintainability**
   - Code readability
   - Function size (should be < 50 lines)
   - Cyclomatic complexity
   - Dependency management
   - Type safety

## Review Template

For each piece of code reviewed, provide:

### Summary
- Overall quality assessment (1-5)
- Key findings count
- Recommended priority areas

### Critical Issues (if any)
- **Issue**: Clear description
- **Location**: File and line number
- **Impact**: Why this matters
- **Severity**: Critical/High/Medium
- **Fix**: Code example

### Findings by Category

#### Security (if issues found)
List security vulnerabilities with examples

#### Performance (if issues found)
List performance problems with complexity analysis

#### Quality (if issues found)
List code quality issues with refactoring suggestions

#### Maintainability (if issues found)
List maintainability problems with improvements
```
## Python 脚本：analyze-metrics.py
```python
#!/usr/bin/env python3
import re
import sys

def analyze_code_metrics(code):
    """Analyze code for common metrics."""

    # Count functions
    functions = len(re.findall(r'^def\s+\w+', code, re.MULTILINE))

    # Count classes
    classes = len(re.findall(r'^class\s+\w+', code, re.MULTILINE))

    # Average line length
    lines = code.split('\n')
    avg_length = sum(len(l) for l in lines) / len(lines) if lines else 0

    # Estimate complexity
    complexity = len(re.findall(r'\b(if|elif|else|for|while|and|or)\b', code))

    return {
        'functions': functions,
        'classes': classes,
        'avg_line_length': avg_length,
        'complexity_score': complexity
    }

if __name__ == '__main__':
    with open(sys.argv[1], 'r') as f:
        code = f.read()
    metrics = analyze_code_metrics(code)
    for key, value in metrics.items():
        print(f"{key}: {value:.2f}")
```
## Python 脚本：compare-complexity.py
```python
#!/usr/bin/env python3
"""
Compare cyclomatic complexity of code before and after changes.
Helps identify if refactoring actually simplifies code structure.
"""

import re
import sys
from typing import Dict, Tuple

class ComplexityAnalyzer:
    """Analyze code complexity metrics."""

    def __init__(self, code: str):
        self.code = code
        self.lines = code.split('\n')

    def calculate_cyclomatic_complexity(self) -> int:
        """
        Calculate cyclomatic complexity using McCabe's method.
        Count decision points: if, elif, else, for, while, except, and, or
        """
        complexity = 1  # Base complexity

        # Count decision points
        decision_patterns = [
            r'\bif\b',
            r'\belif\b',
            r'\bfor\b',
            r'\bwhile\b',
            r'\bexcept\b',
            r'\band\b(?!$)',
            r'\bor\b(?!$)'
        ]

        for pattern in decision_patterns:
            matches = re.findall(pattern, self.code)
            complexity += len(matches)

        return complexity

    def calculate_cognitive_complexity(self) -> int:
        """
        Calculate cognitive complexity - how hard is it to understand?
        Based on nesting depth and control flow.
        """
        cognitive = 0
        nesting_depth = 0

        for line in self.lines:
            # Track nesting depth
            if re.search(r'^\s*(if|for|while|def|class|try)\b', line):
                nesting_depth += 1
                cognitive += nesting_depth
            elif re.search(r'^\s*(elif|else|except|finally)\b', line):
                cognitive += nesting_depth

            # Reduce nesting when unindenting
            if line and not line[0].isspace():
                nesting_depth = 0

        return cognitive

    def calculate_maintainability_index(self) -> float:
        """
        Maintainability Index ranges from 0-100.
        > 85: Excellent
        > 65: Good
        > 50: Fair
        < 50: Poor
        """
        lines = len(self.lines)
        cyclomatic = self.calculate_cyclomatic_complexity()
        cognitive = self.calculate_cognitive_complexity()

        # Simplified MI calculation
        mi = 171 - 5.2 * (cyclomatic / lines) - 0.23 * (cognitive) - 16.2 * (lines / 1000)

        return max(0, min(100, mi))

    def get_complexity_report(self) -> Dict:
        """Generate comprehensive complexity report."""
        return {
            'cyclomatic_complexity': self.calculate_cyclomatic_complexity(),
            'cognitive_complexity': self.calculate_cognitive_complexity(),
            'maintainability_index': round(self.calculate_maintainability_index(), 2),
            'lines_of_code': len(self.lines),
            'avg_line_length': round(sum(len(l) for l in self.lines) / len(self.lines), 2) if self.lines else 0
        }


def compare_files(before_file: str, after_file: str) -> None:
    """Compare complexity metrics between two code versions."""

    with open(before_file, 'r') as f:
        before_code = f.read()

    with open(after_file, 'r') as f:
        after_code = f.read()

    before_analyzer = ComplexityAnalyzer(before_code)
    after_analyzer = ComplexityAnalyzer(after_code)

    before_metrics = before_analyzer.get_complexity_report()
    after_metrics = after_analyzer.get_complexity_report()

    print("=" * 60)
    print("CODE COMPLEXITY COMPARISON")
    print("=" * 60)

    print("\nBEFORE:")
    print(f"  Cyclomatic Complexity:    {before_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {before_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {before_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {before_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {before_metrics['avg_line_length']}")

    print("\nAFTER:")
    print(f"  Cyclomatic Complexity:    {after_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {after_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {after_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {after_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {after_metrics['avg_line_length']}")

    print("\nCHANGES:")
    cyclomatic_change = after_metrics['cyclomatic_complexity'] - before_metrics['cyclomatic_complexity']
    cognitive_change = after_metrics['cognitive_complexity'] - before_metrics['cognitive_complexity']
    mi_change = after_metrics['maintainability_index'] - before_metrics['maintainability_index']
    loc_change = after_metrics['lines_of_code'] - before_metrics['lines_of_code']

    print(f"  Cyclomatic Complexity:    {cyclomatic_change:+d}")
    print(f"  Cognitive Complexity:     {cognitive_change:+d}")
    print(f"  Maintainability Index:    {mi_change:+.2f}")
    print(f"  Lines of Code:            {loc_change:+d}")

    print("\nASSESSMENT:")
    if mi_change > 0:
        print("  ✅ Code is MORE maintainable")
    elif mi_change < 0:
        print("  ⚠️  Code is LESS maintainable")
    else:
        print("  ➡️  Maintainability unchanged")

    if cyclomatic_change < 0:
        print("  ✅ Complexity DECREASED")
    elif cyclomatic_change > 0:
        print("  ⚠️  Complexity INCREASED")
    else:
        print("  ➡️  Complexity unchanged")

    print("=" * 60)


if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python compare-complexity.py <before_file> <after_file>")
        sys.exit(1)

    compare_files(sys.argv[1], sys.argv[2])
```
## 模板：review-checklist.md
```markdown
# Code Review Checklist

## Security Checklist
- [ ] No hardcoded credentials or secrets
- [ ] Input validation on all user inputs
- [ ] SQL injection prevention (parameterized queries)
- [ ] CSRF protection on state-changing operations
- [ ] XSS prevention with proper escaping
- [ ] Authentication checks on protected endpoints
- [ ] Authorization checks on resources
- [ ] Secure password hashing (bcrypt, argon2)
- [ ] No sensitive data in logs
- [ ] HTTPS enforced

## Performance Checklist
- [ ] No N+1 queries
- [ ] Appropriate use of indexes
- [ ] Caching implemented where beneficial
- [ ] No blocking operations on main thread
- [ ] Async/await used correctly
- [ ] Large datasets paginated
- [ ] Database connections pooled
- [ ] Regular expressions optimized
- [ ] No unnecessary object creation
- [ ] Memory leaks prevented

## Quality Checklist
- [ ] Functions < 50 lines
- [ ] Clear variable naming
- [ ] No duplicate code
- [ ] Proper error handling
- [ ] Comments explain WHY, not WHAT
- [ ] No console.logs in production
- [ ] Type checking (TypeScript/JSDoc)
- [ ] SOLID principles followed
- [ ] Design patterns applied correctly
- [ ] Self-documenting code

## Testing Checklist
- [ ] Unit tests written
- [ ] Edge cases covered
- [ ] Error scenarios tested
- [ ] Integration tests present
- [ ] Coverage > 80%
- [ ] No flaky tests
- [ ] Mock external dependencies
- [ ] Clear test names
```
## 模板：finding-template.md

~~~~降价
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
～～～

## 使用示例

~~~~降价
用户：查看此 React 组件代码

Claude：我将使用我的代码审查skills进行全面的代码审查。

[加载skills.md]
[根据清单分析代码]
[运行 Python 指标脚本]
[应用模板]

## 代码审查结果

### 总结
质量得分：3.5/5
主要发现：7 项
优先领域：性能优化、错误处理

### 关键问题：1

**问题**：N+1查询问题
**位置**：UserComponent.tsx，第 45-52 行
**影响**：为 20 个用户的列表呈现 100 多个数据库查询
**严重性**：高
**修复**：
```javascript
// Before: N+1 queries
const users = fetchUsers();
users.forEach(user => fetchUserPosts(user.id)); // 20+ queries

// After: Single query with JOIN
const users = fetchUsersWithPosts(); // 1 query
```
### 性能调查结果
- 大型列表缺少分页
- 建议：对项目使用 React.memo()
- 数据库查询：可以通过索引进行优化

### 质量调查结果
- 第 20 行的函数为 127 行（最多：50 行）
- 缺少错误边界
- Props 应该有 TypeScript 类型
～～～

#### 示例 2：品牌声音技巧

**目录结构：**
```
.claude/skills/brand-voice/
├── SKILL.md
├── brand-guidelines.md
├── tone-examples.md
└── templates/
    ├── email-template.txt
    ├── social-post-template.txt
    └── blog-post-template.md
```
**文件：** `.claude/skills/brand-voice/SKILL.md`
```yaml
---
name: Brand Voice Consistency
description: Ensure all communication matches brand voice and tone guidelines
tags:
  - brand
  - writing
  - consistency
when_to_use: When creating marketing copy, customer communications, or public-facing content
---

# Brand Voice Skill

## Overview
This skill ensures all communications maintain consistent brand voice, tone, and messaging.

## Brand Identity

### Mission
Help teams automate their development workflows with AI

### Values
- **Simplicity**: Make complex things simple
- **Reliability**: Rock-solid execution
- **Empowerment**: Enable human creativity

### Tone of Voice
- **Friendly but professional** - approachable without being casual
- **Clear and concise** - avoid jargon, explain technical concepts simply
- **Confident** - we know what we're doing
- **Empathetic** - understand user needs and pain points

## Writing Guidelines

### Do's ✅
- Use "you" when addressing readers
- Use active voice: "Claude generates reports" not "Reports are generated by Claude"
- Start with value proposition
- Use concrete examples
- Keep sentences under 20 words
- Use lists for clarity
- Include calls-to-action

### Don'ts ❌
- Don't use corporate jargon
- Don't patronize or oversimplify
- Don't use "we believe" or "we think"
- Don't use ALL CAPS except for emphasis
- Don't create walls of text
- Don't assume technical knowledge

## Vocabulary

### ✅ Preferred Terms
- Claude (not "the Claude AI")
- Code generation (not "auto-coding")
- Agent (not "bot")
- Streamline (not "revolutionize")
- Integrate (not "synergize")

### ❌ Avoid Terms
- "Cutting-edge" (overused)
- "Game-changer" (vague)
- "Leverage" (corporate-speak)
- "Utilize" (use "use")
- "Paradigm shift" (unclear)
```
## 示例

### ✅ 好例子
“Claude 可以自动化您的代码审查流程。Claude 无需手动检查每个 PR，而是审查安全性、性能和质量，每周都可以节省您的团队时间。”

为什么有效：明确的价值、具体的好处、以行动为导向

### ❌ 坏榜样
“Claude 利用尖端人工智能提供全面的软件开发解决方案。”

为什么它不起作用：含糊、企业术语、没有具体价值

## 模板：电子邮件
```
Subject: [Clear, benefit-driven subject]

Hi [Name],

[Opening: What's the value for them]

[Body: How it works / What they'll get]

[Specific example or benefit]

[Call to action: Clear next step]

Best regards,
[Name]
```
## 模板：社交媒体
```
[Hook: Grab attention in first line]
[2-3 lines: Value or interesting fact]
[Call to action: Link, question, or engagement]
[Emoji: 1-2 max for visual interest]
```
## 文件：tone-examples.md
```
Exciting announcement:
"Save 8 hours per week on code reviews. Claude reviews your PRs automatically."

Empathetic support:
"We know deployments can be stressful. Claude handles testing so you don't have to worry."

Confident product feature:
"Claude doesn't just suggest code. It understands your architecture and maintains consistency."

Educational blog post:
"Let's explore how agents improve code review workflows. Here's what we learned..."
```
#### 示例 3：文档生成器skills

**文件：** `.claude/skills/doc-generator/SKILL.md`

~~~~yaml
---
名称：API文档生成器
描述：从源代码生成全面、准确的API文档
版本：“1.0.0”
标签：
  - 文档
  - API
  - 自动化
when_to_use：创建或更新 API 文档时
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
  “id”：“usr_123”，
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
const user = wait fetch('/api/v1/users/usr_123', {
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

## Python Script: generate-docs.py

```python
#!/usr/bin/env python3
导入AST
导入 json
从输入导入字典、列表

类 APIDocExtractor(ast.NodeVisitor):
    """从 Python 源代码中提取 API 文档。"""

    def __init__(自身):
        self.端点 = []

    def Visit_FunctionDef(自身, 节点):
        """提取函数文档。"""
        如果node.name.startswith('get_')或node.name.startswith('post_'):
            doc = ast.get_docstring(节点)
            端点={
                '名称'：节点名称，
                '文档字符串'：文档，
                'params': [arg.arg for arg in node.args.args],
                '返回'：self._extract_return_type（节点）
            }
            self.endpoints.append(端点)
        self.generic_visit（节点）

    def _extract_return_type（自身，节点）：
        """从函数注释中提取返回类型。"""
        如果节点返回：
            返回 ast.unparse(node.returns)
        返回“任意”

defgenerate_markdown_docs(endpoints: List[Dict]) -> str:
    """从端点生成降价文档。"""
    docs = "# API 文档\n\n"

    对于端点中的端点：
        docs += f"## {端点['name']}\n\n"
        docs += f"{endpoint['docstring']}\n\n"
        docs += f"**参数**: {', '.join(endpoint['params'])}\n\n"
        文档 += f"**返回**: {endpoint['returns']}\n\n"
        文档 += "---\n\n"

    返回文档

如果 __name__ == '__main__':
    导入系统
    将 open(sys.argv[1], 'r') 作为 f：
        树 = ast.parse(f.read())

    提取器 = APIDocExtractor()
    提取器.访问（树）

    markdown =generate_markdown_docs(extractor.endpoints)
    打印（降价）
～～～
### skills发现与调用
```mermaid
graph TD
    A["User Request"] --> B["Claude Analyzes"]
    B -->|Scans| C["Available Skills"]
    C -->|Metadata check| D["Skill Description Match?"]
    D -->|Yes| E["Load SKILL.md"]
    D -->|No| F["Try next skill"]
    F -->|More skills?| D
    F -->|No more| G["Use general knowledge"]
    E --> H["Extract Instructions"]
    H --> I["Execute Skill"]
    I --> J["Return Results"]
```
### skills与其他功能
```mermaid
graph TB
    A["Extending Claude"]
    B["Slash Commands"]
    C["Subagents"]
    D["Memory"]
    E["MCP"]
    F["Skills"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|User-invoked| G["Quick shortcuts"]
    C -->|Auto-delegated| H["Isolated contexts"]
    D -->|Persistent| I["Cross-session context"]
    E -->|Real-time| J["External data access"]
    F -->|Auto-invoked| K["Autonomous execution"]
```
---

## claude代码Plugins

### 概述

Claude 代码Plugins是使用单个命令安装的自定义项（斜线命令、Subagents、MCP 服务器和hooks）的捆绑集合。它们代表了最高级别的扩展机制——将多个功能组合成有凝聚力的、可共享的包。

＃＃＃ 建筑学
```mermaid
graph TB
    A["Plugin"]
    B["Slash Commands"]
    C["Subagents"]
    D["MCP Servers"]
    E["Hooks"]
    F["Configuration"]

    A -->|bundles| B
    A -->|bundles| C
    A -->|bundles| D
    A -->|bundles| E
    A -->|bundles| F
```
### Plugins加载过程
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as Plugin Marketplace
    participant Install as Installation
    participant SlashCmds as Slash Commands
    participant Subagents
    participant MCPServers as MCP Servers
    participant Hooks
    participant Tools as Configured Tools

    User->>Claude: /plugin install pr-review
    Claude->>Plugin: Download plugin manifest
    Plugin-->>Claude: Return plugin definition
    Claude->>Install: Extract components
    Install->>SlashCmds: Configure
    Install->>Subagents: Configure
    Install->>MCPServers: Configure
    Install->>Hooks: Configure
    SlashCmds-->>Tools: Ready to use
    Subagents-->>Tools: Ready to use
    MCPServers-->>Tools: Ready to use
    Hooks-->>Tools: Ready to use
    Tools-->>Claude: Plugin installed ✅
```
### Plugins类型和分布

|类型 |范围 |共享|权威|示例 |
|------|--------|--------|------------|----------|
|官方|全球|所有用户|人择 |公关审查、安全指南 |
|社区 |公共|所有用户|社区 | DevOps、数据科学 |
|组织|内部|团队成员 |公司 |内部标准、工具|
|个人|个人|单用户|开发商|自定义工作流程 |

### Plugins定义结构
```yaml
---
name: plugin-name
version: "1.0.0"
description: "What this plugin does"
author: "Your Name"
license: MIT

# Plugin metadata
tags:
  - category
  - use-case

# Requirements
requires:
  - claude-code: ">=1.0.0"

# Components bundled
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# Configuration
config:
  auto_load: true
  enabled_by_default: true
---
```
### Plugins结构
```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   ├── skill-1.md
│   └── skill-2.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```
### 实际例子

#### 示例 1：PR 审核Plugins

**文件：** `.claude-plugin/plugin.json`
```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "Complete PR review workflow with security, testing, and docs",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```
**文件：** `commands/review-pr.md`
```markdown
---
name: Review PR
description: Start comprehensive PR review with security and testing checks
---

# PR Review

This command initiates a complete pull request review including:

1. Security analysis
2. Test coverage verification
3. Documentation updates
4. Code quality checks
5. Performance impact assessment
```
**文件：** `agents/security-reviewer.md`
```yaml
---
name: security-reviewer
description: Security-focused code review
tools: read, grep, diff
---

# Security Reviewer

Specializes in finding security vulnerabilities:
- Authentication/authorization issues
- Data exposure
- Injection attacks
- Secure configuration
```
**安装：**
```bash
/plugin install pr-review

# Result:
# ✅ 3 slash commands installed
# ✅ 3 subagents configured
# ✅ 2 MCP servers connected
# ✅ 4 hooks registered
# ✅ Ready to use!
```
#### 示例 2：DevOps Plugins

**组件：**
```
devops-automation/
├── commands/
│   ├── deploy.md
│   ├── rollback.md
│   ├── status.md
│   └── incident.md
├── agents/
│   ├── deployment-specialist.md
│   ├── incident-commander.md
│   └── alert-analyzer.md
├── mcp/
│   ├── github-config.json
│   ├── kubernetes-config.json
│   └── prometheus-config.json
├── hooks/
│   ├── pre-deploy.js
│   ├── post-deploy.js
│   └── on-error.js
└── scripts/
    ├── deploy.sh
    ├── rollback.sh
    └── health-check.sh
```
#### 示例 3：文档Plugins

**捆绑组件：**
```
documentation/
├── commands/
│   ├── generate-api-docs.md
│   ├── generate-readme.md
│   ├── sync-docs.md
│   └── validate-docs.md
├── agents/
│   ├── api-documenter.md
│   ├── code-commentator.md
│   └── example-generator.md
├── mcp/
│   ├── github-docs-config.json
│   └── slack-announce-config.json
└── templates/
    ├── api-endpoint.md
    ├── function-docs.md
    └── adr-template.md
```
### Plugins市场
```mermaid
graph TB
    A["Plugin Marketplace"]
    B["Official<br/>Anthropic"]
    C["Community<br/>Marketplace"]
    D["Enterprise<br/>Registry"]

    A --> B
    A --> C
    A --> D

    B -->|Categories| B1["Development"]
    B -->|Categories| B2["DevOps"]
    B -->|Categories| B3["Documentation"]

    C -->|Search| C1["DevOps Automation"]
    C -->|Search| C2["Mobile Dev"]
    C -->|Search| C3["Data Science"]

    D -->|Internal| D1["Company Standards"]
    D -->|Internal| D2["Legacy Systems"]
    D -->|Internal| D3["Compliance"]
```
### Plugins安装和生命周期
```mermaid
graph LR
    A["Discover"] -->|Browse| B["Marketplace"]
    B -->|Select| C["Plugin Page"]
    C -->|View| D["Components"]
    D -->|Install| E["/plugin install"]
    E -->|Extract| F["Configure"]
    F -->|Activate| G["Use"]
    G -->|Check| H["Update"]
    H -->|Available| G
    G -->|Done| I["Disable"]
    I -->|Later| J["Enable"]
    J -->|Back| G
```
### Plugins功能比较

|特色|斜线命令 |skills|Subagents |Plugins |
|--------|-------------|--------|---------|--------|
| **安装** |手动复制 |手动复制 |手动配置 |一个命令 |
| **设置时间** | 5 分钟 | 10 分钟 | 15 分钟 | 2 分钟 |
| **捆绑** |单文件|单文件|单文件|多个|
| **版本控制** |手册|手册|手册|自动|
| **团队分享** |复制文件|复制文件|复制文件|安装ID |
| **更新** |手册|手册|手册|自动可用 |
| **依赖关系** |无 |无 |无 |可能包括|
| **市场** |没有 |没有 |没有 |是的 |
| **分布** |存储库 |存储库 |存储库 |市场|

### Plugins用例

|使用案例|推荐|为什么 |
|----------|-----------------|-----|
| **团队入职** | ✅ 使用Plugins |即时设置，所有配置 |
| **框架设置** | ✅ 使用Plugins |捆绑特定于框架的命令 |
| **企业标准** | ✅ 使用Plugins |集中分发、版本控制 |
| **快速任务自动化** | ❌ 使用命令 |过于复杂 |
| **单领域专业知识** | ❌使用skills|太重了，改用技巧|
| **专业分析** | ❌ 使用Subagents |手动创建或使用skills |
| **实时数据访问** | ❌ 使用 MCP |独立，请勿捆绑 |

### 何时创建Plugins
```mermaid
graph TD
    A["Should I create a plugin?"]
    A -->|Need multiple components| B{"Multiple commands<br/>or subagents<br/>or MCPs?"}
    B -->|Yes| C["✅ Create Plugin"]
    B -->|No| D["Use Individual Feature"]
    A -->|Team workflow| E{"Share with<br/>team?"}
    E -->|Yes| C
    E -->|No| F["Keep as Local Setup"]
    A -->|Complex setup| G{"Needs auto<br/>configuration?"}
    G -->|Yes| C
    G -->|No| D
```
### 发布Plugins

**发布步骤：**

1. 创建包含所有组件的Plugins结构
2. 写入 `.claude-plugin/plugin.json` 清单
3. 使用文档创建 `README.md`
4. 使用 `/plugin install ./my-plugin` 进行本地测试
5. 提交到Plugins市场
6. 获得审核和批准
7. 在市场上发布
8.用户可以通过一条命令进行安装

**提交示例：**

~~~~降价
# PR 审查Plugins

## 描述
完整的公关审查工作流程，包括安全、测试和文档检查。

## 包含什么
- 3 个斜杠命令适用于不同的评论类型
- 3个专业分agents
- GitHub 和 CodeQL MCP 集成
- 自动安全扫描hooks

## 安装
```bash
/plugin install pr-review
```
## 特点
✅ 安全分析
✅ 测试覆盖率检查
✅ 文件验证
✅ 代码质量评估
✅ 绩效影响分析

## 用法
```bash
/review-pr
/check-security
/check-tests
```
## 要求
- claude代码 1.0+
- GitHub 访问
- CodeQL（可选）
～～～

### Plugins与手动配置

**手动设置（2 小时以上）：**
- 一一安装斜杠命令
- 单独创建Subagents
- 单独配置MCP
- 手动设置hooks
- 记录一切
- 与团队分享（希望他们配置正确）

**使用Plugins（2 分钟）：**
```bash
/plugin install pr-review
# ✅ Everything installed and configured
# ✅ Ready to use immediately
# ✅ Team can reproduce exact setup
```
---

## 比较与整合

### 功能比较矩阵

|特色|调用|坚持|范围 |使用案例|
|--------|---------|------------|--------|---------|
| **斜线命令** |手册 (`/cmd`) |仅限会议 |单一命令 |快捷方式 |
| **Subagents** |自动委派|孤立的背景|专门任务 |任务分配|
| **内存** |自动加载|跨会议 |用户/团队背景|长期学习|
| **MCP 协议** |自动查询|实时外部|实时数据访问 |动态资讯 |
| **skills** |自动调用 |基于文件系统 |可重复使用的专业知识|自动化工作流程 |

### 互动时间轴
```mermaid
graph LR
    A["Session Start"] -->|Load| B["Memory (CLAUDE.md)"]
    B -->|Discover| C["Available Skills"]
    C -->|Register| D["Slash Commands"]
    D -->|Connect| E["MCP Servers"]
    E -->|Ready| F["User Interaction"]

    F -->|Type /cmd| G["Slash Command"]
    F -->|Request| H["Skill Auto-Invoke"]
    F -->|Query| I["MCP Data"]
    F -->|Complex task| J["Delegate to Subagent"]

    G -->|Uses| B
    H -->|Uses| B
    I -->|Uses| B
    J -->|Uses| B
```
### 实际集成示例：客户支持自动化

#### 架构
```mermaid
graph TB
    User["Customer Email"] -->|Receives| Router["Support Router"]

    Router -->|Analyze| Memory["Memory<br/>Customer history"]
    Router -->|Lookup| MCP1["MCP: Customer DB<br/>Previous tickets"]
    Router -->|Check| MCP2["MCP: Slack<br/>Team status"]

    Router -->|Route Complex| Sub1["Subagent: Tech Support<br/>Context: Technical issues"]
    Router -->|Route Simple| Sub2["Subagent: Billing<br/>Context: Payment issues"]
    Router -->|Route Urgent| Sub3["Subagent: Escalation<br/>Context: Priority handling"]

    Sub1 -->|Format| Skill1["Skill: Response Generator<br/>Brand voice maintained"]
    Sub2 -->|Format| Skill2["Skill: Response Generator"]
    Sub3 -->|Format| Skill3["Skill: Response Generator"]

    Skill1 -->|Generate| Output["Formatted Response"]
    Skill2 -->|Generate| Output
    Skill3 -->|Generate| Output

    Output -->|Post| MCP3["MCP: Slack<br/>Notify team"]
    Output -->|Send| Reply["Customer Reply"]
```
#### 请求流程
```markdown
## Customer Support Request Flow

### 1. Incoming Email
"I'm getting error 500 when trying to upload files. This is blocking my workflow!"

### 2. Memory Lookup
- Loads CLAUDE.md with support standards
- Checks customer history: VIP customer, 3rd incident this month

### 3. MCP Queries
- GitHub MCP: List open issues (finds related bug report)
- Database MCP: Check system status (no outages reported)
- Slack MCP: Check if engineering is aware

### 4. Skill Detection & Loading
- Request matches "Technical Support" skill
- Loads support response template from Skill

### 5. Subagent Delegation
- Routes to Tech Support Subagent
- Provides context: customer history, error details, known issues
- Subagent has full access to: read, bash, grep tools

### 6. Subagent Processing
Tech Support Subagent:
- Searches codebase for 500 error in file upload
- Finds recent change in commit 8f4a2c
- Creates workaround documentation

### 7. Skill Execution
Response Generator Skill:
- Uses Brand Voice guidelines
- Formats response with empathy
- Includes workaround steps
- Links to related documentation

### 8. MCP Output
- Posts update to #support Slack channel
- Tags engineering team
- Updates ticket in Jira MCP

### 9. Response
Customer receives:
- Empathetic acknowledgment
- Explanation of cause
- Immediate workaround
- Timeline for permanent fix
- Link to related issues
```
### 完整的功能编排
```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as Memory<br/>CLAUDE.md
    participant MCP as MCP Servers
    participant Skills as Skills
    participant SubAgent as Subagents

    User->>Claude: Request: "Build auth system"
    Claude->>Memory: Load project standards
    Memory-->>Claude: Auth standards, team practices
    Claude->>MCP: Query GitHub for similar implementations
    MCP-->>Claude: Code examples, best practices
    Claude->>Skills: Detect matching Skills
    Skills-->>Claude: Security Review Skill + Testing Skill
    Claude->>SubAgent: Delegate implementation
    SubAgent->>SubAgent: Build feature
    Claude->>Skills: Apply Security Review Skill
    Skills-->>Claude: Security checklist results
    Claude->>SubAgent: Delegate testing
    SubAgent-->>Claude: Test results
    Claude->>User: Complete system delivered
```
### 何时使用每个功能
```mermaid
graph TD
    A["New Task"] --> B{Type of Task?}

    B -->|Repeated workflow| C["Slash Command"]
    B -->|Need real-time data| D["MCP Protocol"]
    B -->|Remember for next time| E["Memory"]
    B -->|Specialized subtask| F["Subagent"]
    B -->|Domain-specific work| G["Skill"]

    C --> C1["✅ Team shortcut"]
    D --> D1["✅ Live API access"]
    E --> E1["✅ Persistent context"]
    F --> F1["✅ Parallel execution"]
    G --> G1["✅ Auto-invoked expertise"]
```
### 选择决策树
```mermaid
graph TD
    Start["Need to extend Claude?"]

    Start -->|Quick repeated task| A{"Manual or Auto?"}
    A -->|Manual| B["Slash Command"]
    A -->|Auto| C["Skill"]

    Start -->|Need external data| D{"Real-time?"}
    D -->|Yes| E["MCP Protocol"]
    D -->|No/Cross-session| F["Memory"]

    Start -->|Complex project| G{"Multiple roles?"}
    G -->|Yes| H["Subagents"]
    G -->|No| I["Skills + Memory"]

    Start -->|Long-term context| J["Memory"]
    Start -->|Team workflow| K["Slash Command +<br/>Memory"]
    Start -->|Full automation| L["Skills +<br/>Subagents +<br/>MCP"]
```
---

## 汇总表

|方面|斜线命令 |Subagents |内存| MCP|skills |Plugins |
|--------|---|---|---|---|---|---|
| **设置困难** |简单|中等|简单|中等|中等|简单|
| **学习曲线** |低|中等|低|中等|中等|低|
| **团队效益** |高|高|中等|高|高|非常高 |
| **自动化水平** |低|高|中等|高|高|非常高 |
| **上下文管理** |单次会议 |隔离|坚持不懈|实时|坚持不懈|所有功能 |
| **维护负担** |低|中等|低|中等|中等|低|
| **可扩展性** |好 |优秀|好 |优秀|优秀|优秀|
| **可分享性** |展会|展会|好 |好 |好 |优秀|
| **版本控制** |手册|手册|手册|手册|手册|自动|
| **安装** |手动复制 |手动配置 |不适用 |手动配置 |手动复制 |一个命令 |

---

## 快速入门指南

### 第 1 周：从简单开始
- 为常见任务创建 2-3 个斜线命令
- 在设置中启用内存
- 在 CLAUDE.md 中记录团队标准

### 第 2 周：添加实时访问
- 设置 1 个 MCP（GitHub 或数据库）
- 使用`/mcp`进行配置
- 查询工作流程中的实时数据

### 第 3 周：分配工作
- 为特定角色创建第一个Subagents
- 使用`/agents`命令
- 通过简单的任务测试委托

### 第 4 周：一切自动化
- 创建第一个重复自动化skills
- 使用skills市场或构建自定义
- 结合所有功能以实现完整的工作流程

### 正在进行中
- 每月回顾和更新内存
- 随着模式的出现添加新skills
- 优化MCP查询
- 完善Subagents提示

---

## Hook

### 概述

hooks是事件驱动的 shell 命令，可自动执行以响应 Claude Code 事件。它们无需人工干预即可实现自动化、验证和自定义工作流程。

### hooks事件

Claude Code 支持四种hooks类型（命令、http、提示、agents）的 **25 个hooks事件**：

|hooks事件 |触发|使用案例 |
|------------|---------|------------|
| **SessionStart** |会话 开始/恢复/清除/紧凑 |环境设置、初始化|
| **InstructionsLoaded** |已加载 CLAUDE.md 或规则文件 |验证、转换、增强 |
| **UserPromptSubmit** |用户提交提示 |输入验证、提示过滤|
| **PreToolUse** |在任何工具运行之前 |验证、批准门、日志记录 |
| **PermissionRequest** |显示权限对话框 |自动批准/拒绝流 |
| **PostToolUse** |工具成功后 |自动格式化、通知、清理 |
| **PostToolUseFailure** |工具执行失败 |错误处理、日志记录 |
| **Notification** |通知已发送 |警报、外部集成 |
| **SubagentStart** |Subagents催生 |上下文注入、初始化 |
| **SubagentStop** |Subagents完成 |结果验证、记录 |
| **Stop** |claude回复完毕 |摘要生成、清理任务 |
| **StopFailure** | API 错误结束回合 |错误恢复、日志记录 |
| **TeammateIdle** |agents队队友闲置|工作分配、协调|
| **TaskCompleted** |任务标记为完成 |任务后处理 |
| **TaskCreated** |通过 TaskCreate | 创建的任务任务跟踪、记录|
| **ConfigChange** |配置文件更改 |验证、传播 |
| **CwdChanged** |工作目录更改 |特定于目录的设置 |
| **FileChanged** |观察文件更改 |文件监控、重建触发器|
| **PreCompact** |上下文压缩之前 |国家保存|
| **PostCompact** |压缩完成后|后紧凑行动|
| **WorktreeCreate** |正在创建工作树 |环境搭建、依赖安装 |
| **WorktreeRemove** |工作树被删除 |清理、资源重新分配 |
| **Elicitation** | MCP 服务器请求用户输入 |输入验证 |
| **ElicitationResult** |用户回应启发 |响应处理 |
| **SessionEnd** |会话终止 |清理、最终记录|

### 常用Hook

hooks在 `~/.claude/settings.json` （用户级）或 `.claude/settings.json` （项目级）中配置：
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```
### hooks环境变量

- `$CLAUDE_FILE_PATH` - 正在编辑/写入的文件的路径
- `$CLAUDE_TOOL_NAME` - 正在使用的工具名称
- `$CLAUDE_SESSION_ID` - 当前会话标识符
- `$CLAUDE_PROJECT_DIR` - 项目目录路径

### 最佳实践

✅ **做：**
- 保持Hook快速（< 1 秒）
- 使用Hook进行验证和自动化
- 优雅地处理错误
- 使用绝对路径

❌ **不要：**
- 使Hook具有交互性
- 对长时间运行的任务使用Hook
- 硬编码凭证

**请参阅**：[06-hooks/](06-hooks/) 了解详细示例

---

## 检查点和倒回

### 概述

检查点允许您保存对话状态并回退到之前的点，从而能够安全地进行多种方法的实验和探索。

### 关键概念

|概念|描述 |
|---------|-------------|
| **检查站** |对话状态快照，包括消息、文件和上下文 |
| **倒带** |返回上一个检查点，放弃后续更改 |
| **分支点** |探索多种方法的检查点 |

### 访问检查点

每次用户提示时都会自动创建检查点。快退：
```bash
# Press Esc twice to open the checkpoint browser
Esc + Esc

# Or use the /rewind command
/rewind
```
选择检查点时，您可以从五个选项中进行选择：
1. **恢复代码和对话** -- 将两者恢复到该点
2. **恢复对话** -- 倒回消息，保留当前代码
3. **恢复代码** -- 恢复文件，保持对话
4. **从这里总结** -- 将对话压缩成摘要
5. **没关系** -- 取消

### 用例

|场景 |工作流程|
|----------|----------|
| **探索方法** |保存→尝试A→保存→倒带→尝试B→比较|
| **安全重构** |保存 → 重构 → 测试 → 如果失败：倒回 |
| **A/B 测试** |保存→设计A→保存→倒带→设计B→比较|
| **错误恢复** |通知问题 → 回到最后的良好状态 |

＃＃＃ 配置
```json
{
  "autoCheckpoint": true
}
```
**请参阅**：[08-checkpoints/](08-checkpoints/) 了解详细示例

---

## 高级功能

### 规划模式

在编码之前制定详细的实施计划。

**激活：**
```bash
/plan Implement user authentication system
```
**好处：**
- 清晰的路线图和时间估计
- 风险评估
- 系统化的任务分解
- 审查和修改的机会

### 延伸思考

对复杂问题的深入推理。

**激活：**
- 在会话期间使用 `Alt+T`（或 macOS 上的 `Option+T`）进行切换
- 设置 `MAX_THINKING_TOKENS` 环境变量以进行编程控制
```bash
# Enable extended thinking via environment variable
export MAX_THINKING_TOKENS=50000
claude -p "Should we use microservices or monolith?"
```
**好处：**
- 全面分析权衡
- 更好的架构决策
- 考虑边缘情况
- 系统评估

### 后台任务

运行长时间操作而不阻塞对话。

**用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # Show all tasks
/task status bg-1234 # Check progress
/task show bg-1234   # View output
/task cancel bg-1234 # Cancel task
```
### 权限模式

控制claude能做什么。

|模式|描述 |使用案例|
|------|-------------|----------|
| **默认** |标准权限，带有敏感操作提示 |总体发展|
| **接受编辑** |自动接受文件编辑而无需确认 |值得信赖的编辑工作流程 |
| **计划** |仅分析和规划，无文件修改 |代码审查、架构规划 |
| **自动** |自动批准安全操作，仅提示有风险的操作 |平衡自主性与安全性|
| **不要问** |执行所有操作，无需确认提示 |经验丰富的用户，自动化 |
| **绕过权限** |完全无限制出入，无安全检查| CI/CD 管道、可信脚本 |

**用法：**
```bash
claude --permission-mode plan          # Read-only analysis
claude --permission-mode acceptEdits   # Auto-accept edits
claude --permission-mode auto          # Auto-approve safe actions
claude --permission-mode dontAsk       # No confirmation prompts
```
### 无头模式（打印模式）

使用 `-p` （打印）标志运行 Claude 代码，无需自动化和 CI/CD 的交互式输入。

**用法：**
```bash
# Run specific task
claude -p "Run all tests"

# Pipe input for analysis
cat error.log | claude -p "explain this error"

# CI/CD integration (GitHub Actions)
- name: AI Code Review
  run: claude -p "Review PR changes and report issues"

# JSON output for scripting
claude -p --output-format json "list all functions in src/"
```
### 计划任务

使用 `/loop` 命令按重复计划运行任务。

**用法：**
```bash
/loop every 30m "Run tests and report failures"
/loop every 2h "Check for dependency updates"
/loop every 1d "Generate daily summary of code changes"
```
计划任务在后台运行并在完成后报告结果。它们对于持续监控、定期检查和自动化维护工作流程非常有用。

### Chrome 集成

Claude Code 可以与 Chrome 浏览器集成以执行 Web 自动化任务。这使得您可以在开发工作流程中直接实现浏览网页、填写表单、截取屏幕截图以及从网站提取数据等功能。

### 会话管理

管理多个工作会话。

**命令：**
```bash
/resume                # Resume a previous conversation
/rename "Feature"      # Name the current session
/fork                  # Fork into a new session
claude -c              # Continue most recent conversation
claude -r "Feature"    # Resume session by name/ID
```
### 互动功能

**键盘快捷键：**
- `Ctrl + R` - 搜索命令历史记录
- `Tab` - 自动完成
- `↑ / ↓` - 命令历史记录
- `Ctrl + L` - 清屏

**多行输入：**
```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```
### 配置

完整配置示例：
```json
{
  "planning": {
    "autoEnter": true,
    "requireApproval": true
  },
  "extendedThinking": {
    "enabled": true,
    "showThinkingProcess": true
  },
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5
  },
  "permissions": {
    "mode": "default"
  }
}
```
**参见**：[09-advanced-features/](09-advanced-features/) 获取综合指南

---

## 资源

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [Anthropic Documentation](https://docs.anthropic.com)
- [MCP GitHub Servers](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---

*最后更新时间：2026 年 3 月*
*适用于claude haiku 4.5、 Sonnet 4.6 和作品 4.6*
*现在包括：hooks、检查点、规划模式、扩展思维、后台任务、权限模式（6 种模式）、无头模式、会话管理、自动记忆、agents团队、计划任务、Chrome 集成、通道、语音听写和捆绑skills*
