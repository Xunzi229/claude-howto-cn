<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 代码功能目录

> 所有 Claude Code 功能的快速参考指南：命令、agents、skills、Plugins和hooks。

**导航**：[Commands](#slash-commands) | [Permission Modes](#permission-modes) | [Subagents](#subagents) | [Skills](#skills) | [Plugins](#plugins) | [MCP Servers](#mcp-servers) | [Hooks](#hooks) | [Memory](#memory-files) | [New Features](#new-features-march-2026)

---

## 总结

|特色|内置|示例 |总计 |参考|
|--------|----------|----------|--------|------------|
| **斜线命令** | 55+ | 8 | 63+ | [01-slash-commands/](01-slash-commands/) |
| **Subagents** | 6 | 10 | 10 16 | 16 [04-subagents/](04-subagents/) |
| **skills** | 5 捆绑 | 4 | 9 | [03-skills/](03-skills/) |
| **Plugins** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 服务器** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **hooks** | 25 场活动 | 7 | 7 | [06-hooks/](06-hooks/) |
| **内存** | 7 种 | 3 | 3 | [02-memory/](02-memory/) |
| **总计** | **99** | **43** | **117** | |

---

## 斜线命令

命令是用户调用的执行特定操作的快捷方式。

### 内置命令

|命令 |描述 |何时使用 |
|---------|-------------|----------|
| `/help` |显示帮助信息 |入门，学习命令 |
| `/btw` |没有添加上下文的附带问题|快速切线问题 |
| `/chrome` |配置 Chrome 集成 |浏览器自动化 |
| `/clear` |清除通话记录 |重新开始，减少背景 |
| `/diff` |交互式差异查看器 |审查变更 |
| `/config` |查看/编辑配置|自定义行为 |
| `/status` |显示会话状态 |检查当前状态 |
| `/agents` |列出可用的agents |查看授权选项 |
| `/skills` |列出可用skills |查看自动调用功能 |
| `/hooks` |列出已配置的Hook |调试自动化|
| `/insights` |分析会话模式 |会话优化 |
| `/install-slack-app` |安装 Claude Slack 应用程序 |松弛集成 |
| `/keybindings` |自定义键盘快捷键 |按键定制|
| `/mcp` |列出 MCP 服务器 |检查外部集成 |
| `/memory` |查看加载的内存文件 |调试上下文加载 |
| `/mobile` |生成手机二维码 |移动访问 |
| `/passes` |查看使用通行证 |订阅信息 |
| `/plugin` |管理Plugins |安装/删除扩展 |
| `/plan` |进入规划模式 |复杂的实施 |
| `/rewind` |倒带至检查点 |撤消更改，探索替代方案 |
| `/checkpoint` |管理检查点 |保存/恢复状态 |
| `/cost` |显示tokens使用成本 |监控支出 |
| `/context` |显示上下文窗口用法 |管理对话长度 |
| `/export` |导出对话 |保存以供参考|
| `/extra-usage` |配置额外的使用限制 |限速管理 |
| `/feedback` |提交反馈或错误报告 |报告问题 |
| `/login` |使用 Anthropic 进行身份验证 |访问功能 |
| `/logout` |退出 |切换账户 |
| `/sandbox` |切换沙盒模式 |安全命令执行 |
| `/vim` |切换 vim 模式 | Vim 风格的编辑 |
| `/doctor` |运行诊断 |解决问题 |
| `/reload-plugins` |重新加载已安装的Plugins |Plugins管理 |
| `/release-notes` |显示发行说明 |检查新功能 |
| `/remote-control` |启用远程控制 |远程访问 |
| `/permissions` |管理权限 |控制访问 |
| `/session` |管理会话 |多会话工作流程 |
| `/rename` |重命名当前会话 |组织会议 |
| `/resume` |恢复上一会话 |继续工作 |
| `/todo` |查看/管理待办事项列表 |跟踪任务 |
| `/tasks` |查看后台任务 |监控异步操作 |
| `/copy` |将上次回复复制到剪贴板 |快速分享输出 |
| `/teleport` |将会话转移到另一台机器 |继续远程工作 |
| `/desktop` |打开claude桌面应用程序|切换到桌面界面 |
| `/theme` |更改颜色主题 |定制外观 |
| `/usage` |显示 API 使用统计信息 |监控配额和成本|
| `/fork` |分叉当前对话 |探索替代方案 |
| `/stats` |显示会话统计信息 |查看会话指标 |
| `/statusline` |配置状态行 |自定义状态显示|
| `/stickers` |查看会话贴纸 |趣味奖励 |
| `/fast` |切换快速输出模式 |加快响应速度 |
| `/terminal-setup` |配置终端集成 |设置终端功能 |
| `/upgrade` |检查更新 |版本管理 |

### 自定义命令（示例）

|命令 |描述 |何时使用 |范围 |安装|
|--------|-------------|-------------|--------|----------------|
| `/optimize` |分析代码以进行优化 |绩效提升|项目| `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` |准备拉取请求 |提交 PR 之前 |项目| `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` |生成API文档|文档 API |项目| `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` |使用上下文创建 git commit |提交更改 |用户 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` |暂存、提交和推送 |快速部署 |用户 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` |重组文档 |改进文档 |项目| `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` |设置 CI/CD 管道 |新项目 |项目| `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` |扩大测试覆盖范围 |改进测试 |项目| `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **范围**：`User` = 个人工作流程 (`~/.claude/commands/`)，`Project` = 团队共享 (`.claude/commands/`)

**参考**：[01-slash-commands/](01-slash-commands/) | [Official Docs](https://code.claude.com/docs/en/interactive-mode)

**快速安装（所有自定义命令）**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```
---

## 权限模式

Claude Code 支持 6 种权限模式，控制工具使用的授权方式。

|模式|描述 |何时使用 |
|------|-------------|-------------|
| `default` |每次工具调用都会提示 |标准交互使用 |
| `acceptEdits` |自动接受文件编辑，提示其他人 |值得信赖的编辑工作流程 |
| `plan` |仅限只读工具，不可写入 |规划与探索|
| `auto` |接受所有工具而不提示 |完全自主运行（研究预览）|
| `bypassPermissions` |跳过所有权限检查 | CI/CD、无头环境 |
| `dontAsk` |跳过需要许可的工具 |非交互式脚本 |

> **注意**：`auto` 模式是一项研究预览功能（2026 年 3 月）。仅在受信任的沙盒环境中使用 `bypassPermissions`。

**参考**：[Official Docs](https://code.claude.com/docs/en/permissions)

---

## Subagents

专门的人工智能助手，具有针对特定任务的隔离上下文。

### 内置Subagents

|agents|描述 |工具|型号|何时使用 |
|--------|-------------|--------|--------|-------------|
| **通用** |多步骤任务、研究 |所有工具|继承模式|复杂的研究、多文件任务 |
| **计划** |实施规划|阅读、Glob、Grep、Bash |继承模式|建筑设计、规划|
| **探索** |代码库探索 |读取、Glob、Grep | haiku 4.5 |快速搜索，理解代码 |
| **Bash** |命令执行 |Bash |继承模式| Git 操作、终端任务 |
| **状态线设置** |状态线配置| Bash、读、写 | Sonnet 4.6 |配置状态行显示 |
| **claude代码指南** |帮助和文档 |读取、Glob、Grep | haiku 4.5 |获取帮助、学习功能 |

### Subagents配置字段

|领域 |类型 |描述 |
|--------|------|-------------|
| `name` |字符串|agents标识符|
| `description` |字符串|agents做什么 |
| `model` |字符串|模型覆盖（例如，`haiku-4.5`）|
| `tools` |数组|允许的工具列表 |
| `effort` |字符串|推理努力水平（`low`、`medium`、`high`）|
| `initialPrompt` |字符串|agents启动时注入系统提示符 |
| `disallowedTools` |数组|明确拒绝此agents使用的工具 |

### 自定义Subagents（示例）

|agents|描述 |何时使用 |范围 |安装|
|--------|-------------|-------------|--------|----------------|
| `code-reviewer` |综合代码质量 |代码审查会议 |项目| `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` |特征架构设计|新功能规划 |项目| `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` |深度代码库分析 |了解现有功能 |项目| `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` |清洁代码原则回顾 |可维护性审查|项目| `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` |测试策略和覆盖范围|测试计划|项目| `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` |技术文档 | API 文档、指南 |项目| `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` |以安全为重点的审查 |安全审计|项目| `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` |全功能实现 |功能开发|项目| `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` |根本原因分析 |错误调查 |用户 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL查询、数据分析|数据任务|用户 | `cp 04-subagents/data-scientist.md .claude/agents/` |

> **范围**：`User` = 个人 (`~/.claude/agents/`)，`Project` = 团队共享 (`.claude/agents/`)

**参考**：[04-subagents/](04-subagents/) | [Official Docs](https://code.claude.com/docs/en/sub-agents)

**快速安装（所有自定义agents）**：
```bash
cp 04-subagents/*.md .claude/agents/
```
---

## skills

带有指令、脚本和模板的自动调用功能。

### skills示例

|skills|描述 |当自动调用时 |范围 |安装|
|--------|-------------|--------------------|--------|----------------|
| `code-review` |全面的代码审查 | “检查此代码”、“检查质量”|项目| `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` |品牌一致性检查器 |撰写营销文案|项目| `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文档生成器 | “生成文档”、“文档 API”|项目| `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` |系统代码重构（Martin Fowler）| “重构这个”，“清理代码” |用户 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **范围**：`User` = 个人 (`~/.claude/skills/`)，`Project` = 团队共享 (`.claude/skills/`)

### skills结构
```
~/.claude/skills/skill-name/
├── SKILL.md          # Skill definition & instructions
├── scripts/          # Helper scripts
└── templates/        # Output templates
```
### skills Frontmatter 字段

skills支持 `SKILL.md` 中的 YAML frontmatter 进行配置：

|领域 |类型 |描述 |
|--------|------|-------------|
| `name` |字符串|skills显示名称|
| `description` |字符串|该skills有什么作用 |
| `autoInvoke` |数组|自动调用的触发短语 |
| `effort` |字符串|推理努力水平（`low`、`medium`、`high`）|
| `shell` |字符串|用于脚本的 shell (`bash`、`zsh`、`sh`) |

**参考**：[03-skills/](03-skills/) | [Official Docs](https://code.claude.com/docs/en/skills)

**快速安装（所有skills）**：
```bash
cp -r 03-skills/* ~/.claude/skills/
```
### 捆绑skills

|skills|描述 |当自动调用时 |
|--------|-------------|--------------------|
| `/simplify` |检查代码质量 |写完代码后|
| `/batch` |对多个文件运行提示 |批量操作 |
| `/debug` |调试失败的测试/错误 |调试会话 |
| `/loop` |按时间间隔运行提示 |重复性任务 |
| `/claude-api` |使用 Claude API 构建应用程序 | API开发 |

---

## Plugins

命令、agents、MCP 服务器和hooks的捆绑集合。

### 示例Plugins

|Plugins |描述 |组件|何时使用 |范围 |安装|
|--------|-------------|------------|-------------|--------|-------------|
| `pr-review` |公关审核工作流程 | 3 个命令，3 个agents，GitHub MCP |代码审查 |项目| `/plugin install pr-review` |
| `devops-automation` |部署与监控 | 4 个命令，3 个agents，K8s MCP | DevOps 任务 |项目| `/plugin install devops-automation` |
| `documentation` |文档生成套件 | 4 个命令、3 个agents、模板 |文档 |项目| `/plugin install documentation` |

> **范围**：`Project` = 团队共享，`User` = 个人工作流程

### Plugins结构
```
.claude-plugin/
├── plugin.json       # Manifest file
├── commands/         # Slash commands
├── agents/           # Subagents
├── skills/           # Skills
├── mcp/              # MCP configurations
├── hooks/            # Hook scripts
└── scripts/          # Utility scripts
```
**参考**：[07-plugins/](07-plugins/) | [Official Docs](https://code.claude.com/docs/en/plugins)

**Plugins管理命令**：
```bash
/plugin list              # List installed plugins
/plugin install <name>    # Install plugin
/plugin remove <name>     # Remove plugin
/plugin update <name>     # Update plugin
```
---

## MCP 服务器

用于外部工具和 API 访问的模型上下文协议服务器。

### 常见 MCP 服务器

|服务器|描述 |何时使用 |范围 |安装|
|--------|-------------|-------------|--------|----------------|
| **GitHub** |公关管理、问题、代码 | GitHub 工作流程 |项目| `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **数据库** | SQL查询、数据访问|数据库操作|项目| `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **文件系统** |高级文件操作|复杂的文件任务 |用户 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** |团队沟通|通知、更新 |项目|在设置|中配置
| **Google Docs** |文档访问 |文档编辑、审核 |项目|在设置|中配置
| **Asana** |项目管理|任务追踪|项目|在设置|中配置
| **Stripe** |付款数据|财务分析|项目|在设置|中配置
| **内存** |持久记忆|跨会话召回 |用户 |在设置|中配置
| **背景7** |库文档 |最新文档查找 |内置|内置|

> **范围**：`Project` = 团队 (`.mcp.json`)、`User` = 个人 (`~/.claude.json`)、`Built-in` = 预安装

### MCP 配置示例
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```
**参考**：[05-mcp/](05-mcp/) | [MCP Protocol Docs](https://modelcontextprotocol.io)

**快速安装 (GitHub MCP)**：
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```
---

## Hook

事件驱动的自动化，对 Claude Code 事件执行 shell 命令。

### hooks事件

|活动 |描述 |何时触发 |使用案例 |
|--------|-------------|----------------|------------------------|
| `SessionStart` |会话 开始/恢复 |会话初始化 |设置任务|
| `InstructionsLoaded` |说明已加载 |已加载 CLAUDE.md 或规则文件 |自定义指令处理 |
| `UserPromptSubmit` |提示处理前 |用户发送消息 |输入验证 |
| `PreToolUse` |工具执行之前 |在任何工具运行之前 |验证、记录|
| `PermissionRequest` |显示权限对话框 |敏感行动之前 |自定义审批流程 |
| `PostToolUse` |工具成功后 |任何工具完成后 |格式化、通知 |
| `PostToolUseFailure` |工具执行失败 |工具出错后 |错误处理、日志记录 |
| `Notification` |通知已发送 |claude发送通知 |外部警报 |
| `SubagentStart` |Subagents催生 |Subagents任务开始 |初始化Subagents上下文 |
| `SubagentStop` |Subagents完成 |Subagents任务完成 |连锁行动|
| `Stop` |claude回复完毕 |回复完成 |清理、报告 |
| `StopFailure` | API 错误结束回合 | API发生错误 |错误恢复、日志记录 |
| `TeammateIdle` |队友agents闲置|agents团队协调 |分配工作 |
| `TaskCompleted` |任务标记为完成 |任务完成 |任务后处理 |
| `TaskCreated` |通过 TaskCreate | 创建的任务新任务已创建 |任务跟踪、记录|
| `ConfigChange` |配置更新 |设置已修改 |对配置更改做出反应 |
| `CwdChanged` |工作目录更改 |目录已更改 |特定于目录的设置 |
| `FileChanged` |观察文件更改 |文件已修改 |文件监控、重建 |
| `PreCompact` |紧凑操作前|上下文压缩 |国家保存|
| `PostCompact` |压缩完成后|压实完成|后紧凑行动|
| `WorktreeCreate` |正在创建工作树 |创建 Git 工作树 |设置worktree环境|
| `WorktreeRemove` |工作树被删除 | Git 工作树已删除 |清理工作树资源 |
| `Elicitation` | MCP 服务器请求输入 | MCP 诱导 |输入验证 |
| `ElicitationResult` |用户回应启发 |用户回应 |响应处理 |
| `SessionEnd` |会话终止 |会话终止 |清理，保存状态|

### hooks示例

|钩|描述 |活动 |范围 |安装|
|------|-------------|--------|--------|----------------|
| `validate-bash.py` |命令验证 |预工具使用：Bash |项目| `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` |安全扫描 | PostTool用途：写入|项目| `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` |自动格式化 | PostTool用途：写入|用户 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` |提示验证 |用户提示提交 |项目| `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` |Token使用跟踪 |停止|用户 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` |预提交验证 |预工具使用：Bash |项目| `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` |命令记录| Post工具使用：Bash |用户 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |

> **范围**：`Project` = 团队 (`.claude/settings.json`)，`User` = 个人 (`~/.claude/settings.json`)

### Hook配置
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```
**参考**：[06-hooks/](06-hooks/) | [Official Docs](https://code.claude.com/docs/en/hooks)

**快速安装（所有hooks）**：
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```
---

## 内存文件

跨会话自动加载持久上下文。

### 内存类型

|类型 |地点 |范围 |何时使用 |
|------|----------|--------|-------------|
| **托管策略** |组织管理的政策 |组织|执行组织范围内的标准 |
| **项目** | `./CLAUDE.md` |项目（团队）|团队标准、项目背景 |
| **项目规则** | `.claude/rules/` |项目（团队）|模块化项目规则|
| **用户** | `~/.claude/CLAUDE.md` |用户（个人）|个人喜好|
| **用户规则** | `~/.claude/rules/` |用户（个人）|模块化个人规则|
| **本地** | `./CLAUDE.local.md` |本地（git 忽略）|特定于机器的覆盖（截至 2026 年 3 月官方文档中尚未出现；可能是遗留的）|
| **自动记忆** |自动|会议|自动捕获的见解和更正|

> **范围**：`Organization` = 由管理员管理，`Project` = 通过 git 与团队共享，`User` = 个人偏好，`Local` = 未提交，`Session` = 自动管理

**参考**：[02-memory/](02-memory/) | [Official Docs](https://code.claude.com/docs/en/memory)

**快速安装**：
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```
---

## 新功能（2026 年 3 月）

|特色|描述 |如何使用 |
|--------|-------------|------------|
| **远程控制** |通过 API 远程控制 Claude Code 会话 |使用远程控制 API 以编程方式发送提示并接收响应 |
| **网络会议** |在基于浏览器的环境中运行 Claude Code |通过 `claude web` 或通过 Anthropic 控制台访问 |
| **桌面应用程序** | Claude Code 的本机桌面应用程序 |使用 `/desktop` 或从 Anthropic 网站下载 |
| **agents团队** |协调多个agents执行相关任务 |配置协作和共享上下文的队友agents |
| **任务清单** |后台任务管理与监控|使用`/tasks`查看和管理后台操作 |
| **及时建议** |上下文感知命令建议 |根据当前上下文自动出现建议 |
| **Git 工作树** |用于并行开发的独立 git 工作树 |使用工作树命令进行安全并行分支工作 |
| **沙盒** |确保安全的隔离执行环境 |使用 `/sandbox` 进行切换；在受限环境中运行命令 |
| **MCP OAuth** | MCP 服务器的 OAuth 身份验证 |在 MCP 服务器设置中配置 OAuth 凭据以实现安全访问 |
| **MCP 工具搜索** |动态搜索和发现 MCP 工具 |使用工具搜索在连接的服务器上查找可用的 MCP 工具 |
| **计划任务** |使用 `/loop` 和 cron 工具设置重复任务 |使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 集成** |使用无头 Chromium 实现浏览器自动化 |使用 `--chrome` 标志或 `/chrome` 命令 |
| **键盘定制** |自定义键绑定，包括和弦支持 |使用 `/keybindings` 或编辑 `~/.claude/keybindings.json` |
| **自动模式** |完全自主运行无需权限提示（研究预览）|使用 `--mode auto` 或 `/permissions auto`； 2026 年 3 月 |
| **频道** |多渠道通信（Telegram、Slack 等）（研究预览）|配置频道Plugins； 2026 年 3 月 |
| **语音听写** |语音输入提示|使用麦克风图标或语音键绑定 |
| **agentshooks类型** |生成Subagents而不是运行 shell 命令的hooks |在Hook配置中设置 `"type": "agent"` |
| **提示hooks类型** |将提示文本注入对话的hooks |在Hook配置中设置 `"type": "prompt"` |
| **MCP 诱导** | MCP 服务器可以在工具执行期间请求用户输入 |通过 `Elicitation` 和 `ElicitationResult` hooks事件处理 |
| **WebSocket MCP 传输** |用于 MCP 服务器连接的基于 WebSocket 的传输在 MCP 服务器配置中使用 `"transport": "websocket"` |
| **Plugins LSP 支持** |通过Plugins集成语言服务器协议 |在 `plugin.json` 中配置 LSP 服务器以获得编辑器功能 |
| **托管Plugins** |组织管理的嵌入式配置 (v2.1.83) |管理员通过托管策略进行配置；自动应用于所有用户 |

---

## 快速参考矩阵

### 功能选择指南

|需要|推荐功能 |为什么 |
|------|---------------------|-----|
|快捷捷径|斜线命令 |手动，即时 |
|持久上下文 |内存|自动加载|
|复杂自动化|skills|自动调用 |
|专门任务 |Subagents |孤立的背景|
|外部数据| MCP 服务器 |实时访问 |
|事件自动化|hooks|事件触发|
|完整的解决方案|Plugins |多合一捆绑包 |

### 安装优先级

|优先|特色|命令 |
|----------|---------|---------|
| 1. 必备 |内存| `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用|斜线命令 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 品质 |Subagents | `cp 04-subagents/*.md .claude/agents/` |
| 4.自动化|hooks| `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5.外部| MCP| `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 进阶|skills | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完成 |Plugins | `/plugin install pr-review` |

---

## 完成一命令安装

安装此存储库中的所有示例：
```bash
# Create directories
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# Install all features
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```
---

## 其他资源

- [Official Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Learning Roadmap](LEARNING-ROADMAP.md)
- [Main README](README.md)

---

**最后更新**：2026 年 3 月
