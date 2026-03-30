# 变更日志

## v2.2.0 — 2026-03-26

### 文档

- 将所有教程和参考与 Claude Code v2.1.84 (f78c094) @luongnv89 同步
  - 将斜杠命令更新为 55+ 内置 + 5 项捆绑skills，标记 3 项已弃用
  - 将Hook事件从18个扩展到25个，添加`agent`Hook类型（现在有4种）
  - 添加自动模式、频道、语音听写等高级功能
  - 添加`effort`、`shell`skills前题字段； `initialPrompt`、`disallowedTools` agents字段
  - 添加 WebSocket MCP 传输、启发、2KB 工具上限
  - 添加Plugins LSP 支持，`userConfig`，`${CLAUDE_PLUGIN_DATA}`
  - 更新所有参考文档（CATALOG、QUICK_REFERENCE、LEARNING-ROADMAP、INDEX）
- 将自述文件重写为登陆页面结构指南 (32a0776) @luongnv89

### 错误修复

- 添加缺失的 cSpell 单词和自述文件部分以实现 CI 合规性 (93f9d51) @luongnv89
- 将 `Sandboxing` 添加到 cSpell 字典 (b80ce6f) @luongnv89

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### 错误修复

- 删除未通过 CI 链接检查的无效市场链接 (3fdf0d6) @luongnv89
- 将 `sandboxed` 和 `pycache` 添加到 cSpell 字典 (dc64618) @luongnv89

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 特点

- 添加具有自我评估和课程测验skills的自适应学习路径（1ef46cd）@luongnv89
  - `/self-assessment` — 跨越 10 个功能领域的互动能力测验，提供个性化学习路径
  - `/lesson-quiz [lesson]` — 每课知识检查，包含 8-10 个有针对性的问题

### 错误修复

- 更新损坏的 URL、弃用和过时的引用 (8fe4520) @luongnv89
- 修复资源和自我评估skills中损坏的链接（7a05863）@luongnv89
- 在概念指南中使用波浪号栅栏来嵌套代码块（5f82719）@VikalpP
- 将缺失的单词添加到 cSpell 词典 (8df7572) @luongnv89

### 文档

- 第 5 阶段 QA — 修复文档之间的一致性、URL 和术语 (00bbe4c) @luongnv89
- 完成第 3-4 阶段 — 新功能覆盖范围和参考文档更新 (132de29) @luongnv89
- 将 MCPorter 运行时添加到 MCP 上下文膨胀部分 (ef52705) @luongnv89
- 在 6 个指南中添加缺少的命令、功能和设置 (4bc8f15) @luongnv89
- 添加基于现有存储库约定的样式指南 (84141d0) @luongnv89
- 添加自我评估行到指南比较表（8fe0c96）@luongnv89
- 将 VikalpP 添加到 PR #7 (d5b4350) @luongnv89 的贡献者列表
- 在自述文件和路线图中添加自我评估和课程测验skills参考（d5a6106）@luongnv89

### 新贡献者

- @VikalpP 在#7 中做出了第一个贡献

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 特点

- 将所有文档与 Claude Code February 2026 功能同步 (487c96d)
  - 更新了所有 10 个教程目录和 7 个参考文档中的 26 个文件
  - 添加**自动记忆**文档 - 每个项目的持续学习
  - 添加**远程控制**、**网络会话**和**桌面应用程序**的文档
  - 添加 **agents团队** 的文档（实验性多agents协作）
  - 添加 **MCP OAuth 2.0**、**工具搜索**和 **Claude.ai 连接器**的文档
  - 为Subagents添加**持久内存**和**工作树隔离**的文档
  - 添加**后台Subagents**、**任务列表**、**提示建议**的文档
  - 添加**沙盒**和**托管设置**（企业）的文档
  - 添加 **HTTP Hooks** 和 7 个新hooks事件的文档
  - 添加**Plugins设置**、**LSP 服务器**和市场更新的文档
  - 添加 **从检查点总结** 倒回选项的文档
  - 记录 17 个新斜杠命令（`/fork`、`/desktop`、`/teleport`、`/tasks`、`/fast` 等）
  - 记录新的 CLI 标志（`--worktree`、`--from-pr`、`--remote`、`--teleport`、`--teammate-mode` 等）
  - 记录自动记忆、工作水平、agents团队等的新环境变量

### 设计

- 使用最小调色板将徽标重新设计为罗盘括号标记（20779db）

### 错误修复/更正

- 更新型号名称：Sonnet 4.5 → **Sonnet 4.6**、Opus 4.5 → **Opus 4.6**
- 修复权限模式名称：将虚构的“无限制/确认/只读”替换为实际的 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- 修复hooks事件：删除虚构的 `PreCommit`/`PostCommit`/`PrePush`，添加真实事件（`SubagentStart`、`WorktreeCreate`、`ConfigChange` 等）
- 修复 CLI 语法：将 `claude-code --headless` 替换为 `claude -p`（打印模式）
- 修复检查点命令：用实际的 `Esc+Esc` / `/rewind` 接口替换虚构的 `/checkpoint save/list/rewind/diff`
- 修复会话管理：用真实的 `/resume`/`/rename`/`/fork` 替换虚构的 `/session list/new/switch/save`
- 修复Plugins清单格式：迁移 `plugin.yaml` → `.claude-plugin/plugin.json`
- 修复 MCP 配置路径：`~/.claude/mcp.json` → `.mcp.json`（项目）/`~/.claude.json`（用户）
- 修复文档 URL：`docs.claude.com` → `docs.anthropic.com`；删除虚构的 `plugins.claude.com`
- 删除多个文件中的虚构配置字段
- 将所有“上次更新”日期更新为 2026 年 2 月

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
