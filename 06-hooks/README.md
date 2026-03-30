<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Hook

hooks是自动脚本，用于响应 Claude Code 会话期间的特定事件而执行。它们支持自动化、验证、权限管理和自定义工作流程。

## 概述

hooks是自动操作（shell 命令、HTTP webhooks、LLM 提示或Subagents评估），当 Claude Code 中发生特定事件时自动执行。它们接收 JSON 输入并通过退出代码和 JSON 输出传达结果。

**主要特点：**
- 事件驱动的自动化
- 基于 JSON 的输入/输出
- 支持命令、提示符、HTTP 和agentshooks类型
- 特定工具hooks的模式匹配

## 配置

hooks在具有特定结构的设置文件中配置：

- `~/.claude/settings.json` - 用户设置（所有项目）
- `.claude/settings.json` - 项目设置（可共享、已提交）
- `.claude/settings.local.json` - 本地项目设置（未提交）
- 托管策略 - 组织范围的设置
- Plugins `hooks/hooks.json` - Plugins范围的Hook
- skills/agents frontmatter - 组件生命周期hooks

### 基本配置结构
```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```
**关键字段：**

|领域 |描述 |示例|
|--------|-------------|---------|
| `matcher` |匹配工具名称的模式（区分大小写）| `"Write"`、`"Edit\|Write"`、`"*"` |
| `hooks` |Hook定义数组 | `[{ "type": "command", ... }]` |
| `type` |hooks类型：`"command"` (bash)、`"prompt"` (LLM)、`"http"` (webhook) 或 `"agent"` (Subagents) | `"command"` |
| `command` |执行的 Shell 命令 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` |可选超时秒数（默认 60） | `30` |
| `once` |如果 `true`，则每个会话仅运行一次Hook | `true` |

### 匹配器模式

|图案|描述 |示例|
|---------|-------------|---------|
|精确字符串|匹配特定工具 | `"Write"` |
|正则表达式模式 |匹配多种工具| `"Edit\|Write"` |
|通配符|匹配所有工具 | `"*"` 或 `""` |
| MCP 工具 |服务器和工具模式| `"mcp__memory__.*"` |

## Hook类型

Claude Code 支持四种Hook类型：

### 命令hooks

默认的Hook类型。执行 shell 命令并通过 JSON stdin/stdout 和退出代码进行通信。
```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```
### HTTP hooks

> v2.1.63 中添加。

接收与命令hooks相同的 JSON 输入的远程 Webhook 端点。 HTTP 将 POST JSON hooks到 URL 并接收 JSON 响应。当启用沙箱时，HTTP hooks将通过沙箱进行路由。为了安全起见，URL 中的环境变量插值需要显式 `allowedEnvVars` 列表。
```json
{
  "hooks": {
    "PostToolUse": [{
      "type": "http",
      "url": "https://my-webhook.example.com/hook",
      "matcher": "Write"
    }]
  }
}
```
**关键属性：**
- `"type": "http"` -- 将其标识为 HTTP hooks
- `"url"` -- webhook 端点 URL
- 启用沙箱时通过沙箱路由
- URL 中的任何环境变量插值都需要显式 `allowedEnvVars` 列表

### 提示Hook

LLM 评估的提示，其中hooks内容是 Claude 评估的提示。主要与 `Stop` 和 `SubagentStop` 事件一起用于智能任务完成检查。
```json
{
  "type": "prompt",
  "prompt": "Evaluate if Claude completed all requested tasks.",
  "timeout": 30
}
```
LLM 评估提示并返回结构化决策（有关详细信息，请参阅 [Prompt-Based Hooks](#prompt-based-hooks)）。

### agentshooks

基于Subagents的验证hooks，生成专用agents来评估条件或执行复杂的检查。与提示Hook（单轮LLM评估）不同，agentsHook可以使用工具并执行多步骤推理。
```json
{
  "type": "agent",
  "prompt": "Verify the code changes follow our architecture guidelines. Check the relevant design docs and compare.",
  "timeout": 120
}
```
**关键属性：**
- `"type": "agent"` -- 将其标识为agentshooks
- `"prompt"` -- Subagents的任务描述
- agents可以使用工具（Read、Grep、Bash 等）来执行其评估
- 返回类似于提示Hook的结构化决策

## hooks事件

Claude Code 支持 **25 个Hook事件**：

|事件 |何时触发 |匹配器输入|可以阻止|常见用途 |
|--------|-------------|----------------|------------|------------|
| **SessionStart** |会话 开始/恢复/清除/紧凑 |启动/恢复/清除/紧凑|没有 |环境设置|
| **InstructionsLoaded** | CLAUDE.md 或规则文件加载后 | （无）|没有 |修改/过滤指令|
| **UserPromptSubmit** |用户提交提示 | （无）|是的 |验证提示 |
| **PreToolUse** |工具执行之前 |工具名称 |是（允许/拒绝/询问）|验证、修改输入 |
| **PermissionRequest** |显示权限对话框 |工具名称|是的 |自动批准/拒绝 |
| **PostToolUse** |工具成功后 |工具名称 |没有 |添加上下文、反馈 |
| **PostToolUseFailure** |工具执行失败 |工具名称|没有 |错误处理、日志记录 |
| **Notification** |通知已发送 |通知类型 |没有 |自定义通知 |
| **SubagentStart** |Subagents催生 |agents类型名称 |没有 |Subagents设置 |
| **SubagentStop** |Subagents完成 |agents类型名称 |是的 |Subagents验证 |
| **Stop** |claude回复完毕 | （无）|是的 |任务完成情况检查|
| **StopFailure** | API 错误结束回合 | （无）|没有 |错误恢复、日志记录 |
| **TeammateIdle** |agents队队友闲置| （无）|是的 |队友配合|
| **TaskCompleted** |任务标记为完成 | （无）|是的 |任务后行动 |
| **TaskCreated** |通过 TaskCreate | 创建的任务（无）|没有 |任务跟踪、记录|
| **ConfigChange** |配置文件更改 | （无）|是（政策除外）|对配置更新做出反应 |
| **CwdChanged** |工作目录更改 | （无）|没有 |特定于目录的设置 |
| **FileChanged** |观察文件更改 | （无）|没有 |文件监控、重建 |
| **PreCompact** |上下文压缩之前 |手动/自动 |没有 |预压缩行动|
| **PostCompact** |压缩完成后| （无）|没有 |后紧凑行动|
| **WorktreeCreate** |正在创建工作树 | （无）|是（路径返回）|工作树初始化 |
| **WorktreeRemove** |工作树被删除 | （无）|没有 |工作树清理 |
| **Elicitation** | MCP 服务器请求用户输入 | （无）|是的 |输入验证 |
| **ElicitationResult** |用户回应启发 | （无）|是的 |响应处理 |
| **SessionEnd** |会话终止 | （无）|没有 |清理、最终记录|

### PreToolUse

在 Claude 创建工具参数之后、处理之前运行。使用它来验证或修改工具输入。

**配置：**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py"
          }
        ]
      }
    ]
  }
}
```
**常用匹配器：** `Task`、`Bash`、`Glob`、`Grep`、`Read`、`Edit`、`Write`、`WebFetch`、`WebSearch`

**输出控制：**
- `permissionDecision`：`"allow"`、`"deny"` 或 `"ask"`
- `permissionDecisionReason`：决定的解释
- `updatedInput`：修改工具输入参数

### PostToolUse

工具完成后立即运行。用于验证、记录或向 Claude 提供上下文。

**配置：**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py"
          }
        ]
      }
    ]
  }
}
```
**输出控制：**
- `"block"` 决策提示claude提供反馈
- `additionalContext`：为claude添加了上下文

### UserPromptSubmit

当用户提交提示时运行，然后 Claude 处理它。

**配置：**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py"
          }
        ]
      }
    ]
  }
}
```
**输出控制：**
- `decision`: `"block"` 阻止处理
- `reason`：如果被阻止则说明
- `additionalContext`：添加到提示中的上下文

### Stop and SubagentStop

当 Claude 完成响应 (Stop) 或Subagents完成 (SubagentStop) 时运行。支持基于提示的评估，以智能检查任务完成情况。

**附加输入字段：** `Stop` 和 `SubagentStop` hooks都会在其 JSON 输入中接收 `last_assistant_message` 字段，其中包含停止前来自 Claude 或Subagents的最终消息。这对于评估任务完成情况很有用。

**配置：**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate if Claude completed all requested tasks.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```
### SubagentStart

当Subagents开始执行时运行。匹配器输入是agents类型名称，允许hooks针对特定的Subagents类型。

**配置：**
```json
{
  "hooks": {
    "SubagentStart": [
      {
        "matcher": "code-review",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/subagent-init.sh"
          }
        ]
      }
    ]
  }
}
```
### SessionStart

在会话开始或恢复时运行。可以持久化环境变量。

**匹配器：** `startup`、`resume`、`clear`、`compact`

**特殊功能：** 使用 `CLAUDE_ENV_FILE` 保存环境变量（也可在 `CwdChanged` 和 `FileChanged` hooks中使用）：
```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```
### SessionEnd

在会话结束时运行以执行清理或最终日志记录。无法阻止终止。

**原因字段值：**
- `clear` - 用户清除会话
- `logout` - 用户已注销
- `prompt_input_exit` - 用户通过提示输入退出
- `other` - 其他原因

**配置：**
```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-cleanup.sh\""
          }
        ]
      }
    ]
  }
}
```
### Notification Event

更新了通知事件的匹配器：
- `permission_prompt` - 权限请求通知
- `idle_prompt` - 空闲状态通知
- `auth_success` - 身份验证成功
- `elicitation_dialog` - 向用户显示的对话框

## Component-Scoped Hooks

hooks可以附加到其 frontmatter 中的特定组件（skills、agents、命令）：

**在 SKILL.md、agent.md 或 command.md 中：**
```yaml
---
name: secure-operations
description: Perform operations with security checks
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/check.sh"
          once: true  # Only run once per session
---
```
**组件Hook支持的事件：** `PreToolUse`、`PostToolUse`、`Stop`

这允许直接在使用Hook的组件中定义Hook，将相关代码放在一起。

### Hooks in Subagent Frontmatter

当在Subagents的 frontmatter 中定义 `Stop` hooks时，它会自动转换为作用域为该Subagents的 `SubagentStop` hooks。这确保停止hooks仅在特定Subagents完成时触发，而不是在主会话停止时触发。
```yaml
---
name: code-review-agent
description: Automated code review subagent
hooks:
  Stop:
    - hooks:
        - type: prompt
          prompt: "Verify the code review is thorough and complete."
  # The above Stop hook auto-converts to SubagentStop for this subagent
---
```
## PermissionRequest Event

使用自定义输出格式处理权限请求：
```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "Custom message",
      "interrupt": false
    }
  }
}
```
## Hook Input and Output

### JSON 输入（通过标准输入）

所有Hook都通过 stdin 接收 JSON 输入：
```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Write",
  "tool_input": {
    "file_path": "/path/to/file.js",
    "content": "..."
  },
  "tool_use_id": "toolu_01ABC123...",
  "agent_id": "agent-abc123",
  "agent_type": "main",
  "worktree": "/path/to/worktree"
}
```
**常用字段：**

|领域 |描述 |
|--------|-------------|
| `session_id` |唯一会话标识符 |
| `transcript_path` |对话记录文件的路径 |
| `cwd` |当前工作目录 |
| `hook_event_name` |触发hooks的事件名称 |
| `agent_id` |运行此Hook的agents的标识符 |
| `agent_type` |agents类型（`"main"`、Subagents类型名称等）|
| `worktree` | git 工作树的路径（如果agents在一个 | 中运行）

### 退出代码

|退出代码 |意义|行为 |
|------------|---------|----------|
| **0** |成功|继续，解析 JSON 标准输出 |
| **2** |阻塞错误|块操作，stderr 显示为错误 |
| **其他** |非阻塞错误 |继续，stderr 以详细模式显示 |

### JSON 输出（标准输出，退出代码 0）
```json
{
  "continue": true,
  "stopReason": "Optional message if stopping",
  "suppressOutput": false,
  "systemMessage": "Optional warning message",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "File is in allowed directory",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```
## 环境变量

|变量|可用性 |描述 |
|----------|-------------|-------------|
| `CLAUDE_PROJECT_DIR` |所有hooks|项目根目录的绝对路径 |
| `CLAUDE_ENV_FILE` |会话 开始、CwdChanged、文件更改 |持久化环境变量的文件路径 |
| `CLAUDE_CODE_REMOTE` |所有hooks | `"true"` 如果在远程环境中运行 |
| `${CLAUDE_PLUGIN_ROOT}` |Pluginshooks |Plugins目录路径 |
| `${CLAUDE_PLUGIN_DATA}` |Pluginshooks |Plugins数据目录的路径 |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hooks | SessionEnd hooks的可配置超时（以毫秒为单位）（覆盖默认值）|

## 基于提示的Hook

对于 `Stop` 和 `SubagentStop` 事件，您可以使用基于 LLM 的评估：
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if all tasks are complete. Return your decision.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```
**LLM Response Schema：**
```json
{
  "decision": "approve",
  "reason": "All tasks completed successfully",
  "continue": false,
  "stopReason": "Task complete"
}
```
## 示例

### 示例 1：Bash 命令验证器 (PreToolUse)

**文件：** `.claude/hooks/validate-bash.py`
```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"\brm\s+-rf\s+/", "Blocking dangerous rm -rf / command"),
    (r"\bsudo\s+rm", "Blocking sudo rm command"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name != "Bash":
        sys.exit(0)

    command = input_data.get("tool_input", {}).get("command", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, command):
            print(message, file=sys.stderr)
            sys.exit(2)  # Exit 2 = blocking error

    sys.exit(0)

if __name__ == "__main__":
    main()
```
**配置：**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\""
          }
        ]
      }
    ]
  }
}
```
### 示例 2：安全扫描程序 (PostToolUse)

**文件：** `.claude/hooks/security-scan.py`
```python
#!/usr/bin/env python3
import json
import sys
import re

SECRET_PATTERNS = [
    (r"password\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded password"),
    (r"api[_-]?key\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded API key"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name not in ["Write", "Edit"]:
        sys.exit(0)

    tool_input = input_data.get("tool_input", {})
    content = tool_input.get("content", "") or tool_input.get("new_string", "")
    file_path = tool_input.get("file_path", "")

    warnings = []
    for pattern, message in SECRET_PATTERNS:
        if re.search(pattern, content, re.IGNORECASE):
            warnings.append(message)

    if warnings:
        output = {
            "hookSpecificOutput": {
                "hookEventName": "PostToolUse",
                "additionalContext": f"Security warnings for {file_path}: " + "; ".join(warnings)
            }
        }
        print(json.dumps(output))

    sys.exit(0)

if __name__ == "__main__":
    main()
```
### 示例 3：自动格式化代码 (PostToolUse)

**文件：** `.claude/hooks/format-code.sh`
```bash
#!/bin/bash

# Read JSON from stdin
INPUT=$(cat)
TOOL_NAME=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_name', ''))")
FILE_PATH=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_input', {}).get('file_path', ''))")

if [ "$TOOL_NAME" != "Write" ] && [ "$TOOL_NAME" != "Edit" ]; then
    exit 0
fi

# Format based on file extension
case "$FILE_PATH" in
    *.js|*.jsx|*.ts|*.tsx|*.json)
        command -v prettier &>/dev/null && prettier --write "$FILE_PATH" 2>/dev/null
        ;;
    *.py)
        command -v black &>/dev/null && black "$FILE_PATH" 2>/dev/null
        ;;
    *.go)
        command -v gofmt &>/dev/null && gofmt -w "$FILE_PATH" 2>/dev/null
        ;;
esac

exit 0
```
### 示例 4：提示验证器 (UserPromptSubmit)

**文件：** `.claude/hooks/validate-prompt.py`
```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"delete\s+(all\s+)?database", "Dangerous: database deletion"),
    (r"rm\s+-rf\s+/", "Dangerous: root deletion"),
]

def main():
    input_data = json.load(sys.stdin)
    prompt = input_data.get("user_prompt", "") or input_data.get("prompt", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, prompt, re.IGNORECASE):
            output = {
                "decision": "block",
                "reason": f"Blocked: {message}"
            }
            print(json.dumps(output))
            sys.exit(0)

    sys.exit(0)

if __name__ == "__main__":
    main()
```
### 示例 5：智能停止hooks（基于提示）
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if Claude completed all requested tasks. Check: 1) Were all files created/modified? 2) Were there unresolved errors? If incomplete, explain what's missing.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```
### 示例 6：上下文使用跟踪器（Hook对）

使用 `UserPromptSubmit` （消息前）和 `Stop` （响应后）hooks来跟踪每个请求的Token消耗。

**文件：** `.claude/hooks/context-tracker.py`
```python
#!/usr/bin/env python3
"""
Context Usage Tracker - Tracks token consumption per request.

Uses UserPromptSubmit as "pre-message" hook and Stop as "post-response" hook
to calculate the delta in token usage for each request.

Token Counting Methods:
1. Character estimation (default): ~4 chars per token, no dependencies
2. tiktoken (optional): More accurate (~90-95%), requires: pip install tiktoken
"""
import json
import os
import sys
import tempfile

# Configuration
CONTEXT_LIMIT = 128000  # Claude's context window (adjust for your model)
USE_TIKTOKEN = False    # Set True if tiktoken is installed for better accuracy


def get_state_file(session_id: str) -> str:
    """Get temp file path for storing pre-message token count, isolated by session."""
    return os.path.join(tempfile.gettempdir(), f"claude-context-{session_id}.json")


def count_tokens(text: str) -> int:
    """
    Count tokens in text.

    Uses tiktoken with p50k_base encoding if available (~90-95% accuracy),
    otherwise falls back to character estimation (~80-90% accuracy).
    """
    if USE_TIKTOKEN:
        try:
            import tiktoken
            enc = tiktoken.get_encoding("p50k_base")
            return len(enc.encode(text))
        except ImportError:
            pass  # Fall back to estimation

    # Character-based estimation: ~4 characters per token for English
    return len(text) // 4


def read_transcript(transcript_path: str) -> str:
    """Read and concatenate all content from transcript file."""
    if not transcript_path or not os.path.exists(transcript_path):
        return ""

    content = []
    with open(transcript_path, "r") as f:
        for line in f:
            try:
                entry = json.loads(line.strip())
                # Extract text content from various message formats
                if "message" in entry:
                    msg = entry["message"]
                    if isinstance(msg.get("content"), str):
                        content.append(msg["content"])
                    elif isinstance(msg.get("content"), list):
                        for block in msg["content"]:
                            if isinstance(block, dict) and block.get("type") == "text":
                                content.append(block.get("text", ""))
            except json.JSONDecodeError:
                continue

    return "\n".join(content)


def handle_user_prompt_submit(data: dict) -> None:
    """Pre-message hook: Save current token count before request."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Save to temp file for later comparison
    state_file = get_state_file(session_id)
    with open(state_file, "w") as f:
        json.dump({"pre_tokens": current_tokens}, f)


def handle_stop(data: dict) -> None:
    """Post-response hook: Calculate and report token delta."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Load pre-message count
    state_file = get_state_file(session_id)
    pre_tokens = 0
    if os.path.exists(state_file):
        try:
            with open(state_file, "r") as f:
                state = json.load(f)
                pre_tokens = state.get("pre_tokens", 0)
        except (json.JSONDecodeError, IOError):
            pass

    # Calculate delta
    delta_tokens = current_tokens - pre_tokens
    remaining = CONTEXT_LIMIT - current_tokens
    percentage = (current_tokens / CONTEXT_LIMIT) * 100

    # Report usage
    method = "tiktoken" if USE_TIKTOKEN else "estimated"
    print(f"Context ({method}): ~{current_tokens:,} tokens ({percentage:.1f}% used, ~{remaining:,} remaining)", file=sys.stderr)
    if delta_tokens > 0:
        print(f"This request: ~{delta_tokens:,} tokens", file=sys.stderr)


def main():
    data = json.load(sys.stdin)
    event = data.get("hook_event_name", "")

    if event == "UserPromptSubmit":
        handle_user_prompt_submit(data)
    elif event == "Stop":
        handle_stop(data)

    sys.exit(0)


if __name__ == "__main__":
    main()
```
**配置：**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ]
  }
}
```
**它是如何工作的：**
1. `UserPromptSubmit` 在处理提示之前触发 - 保存当前Token计数
2. `Stop` 在 Claude 响应后触发 - 计算增量并报告使用情况
3. 每个会话通过临时文件名中的 `session_id` 进行隔离

**Token计数方法：**

|方法|准确度|依赖关系 |速度|
|--------|----------|--------------|--------|
|人物估计| 〜80-90% |无 | <1ms |
| tiktoken (p50k_base) | ~90-95% | `pip install tiktoken` | <10ms |

> **注意：** Anthropic 尚未发布官方离线分词器。两种方法都是近似值。文字记录包括用户提示、Claude 的响应和工具输出，但不包括系统提示或内部上下文。

### 示例 7：预置 auto-mode 权限（一次性设置脚本）

这是一个一次性设置脚本，用于把大约 67 条安全权限规则预置到 `~/.claude/settings.json` 中，效果相当于 Claude Code's auto-mode 基线权限。它不依赖 hooks，也不会记住你后续的选择。运行一次即可；重复运行也安全，因为已经存在的规则会被跳过。

**文件：** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 预览将要添加的规则
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 正式写入
python3 09-advanced-features/setup-auto-mode-permissions.py
```

**会添加哪些内容：**

| 类别 | 示例 |
|----------|---------|
| 内置工具 | `Read(*)`、`Edit(*)`、`Write(*)`、`Glob(*)`、`Grep(*)`、`Agent(*)`、`WebSearch(*)` |
| Git 只读 | `Bash(git status:*)`、`Bash(git log:*)`、`Bash(git diff:*)` |
| Git 写入（本地） | `Bash(git add:*)`、`Bash(git commit:*)`、`Bash(git checkout:*)` |
| 包管理器 | `Bash(npm install:*)`、`Bash(pip install:*)`、`Bash(cargo build:*)` |
| 构建与测试 | `Bash(make:*)`、`Bash(pytest:*)`、`Bash(go test:*)` |
| 常用 shell | `Bash(ls:*)`、`Bash(cat:*)`、`Bash(find:*)`、`Bash(cp:*)`、`Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`、`Bash(gh pr create:*)`、`Bash(gh issue list:*)` |

**有意排除的内容**（这个脚本绝不会添加）：
- `rm -rf`、`sudo`、强制 push、`git reset --hard`
- `DROP TABLE`、`kubectl delete`、`terraform destroy`
- `npm publish`、`curl | bash`、生产环境 deploy

## Pluginshooks

Plugins可以在其 `hooks/hooks.json` 文件中包含Hook：

**文件：** `plugins/hooks/hooks.json`
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/validate.sh"
          }
        ]
      }
    ]
  }
}
```
**Pluginshooks中的环境变量：**
- `${CLAUDE_PLUGIN_ROOT}` - Plugins目录的路径
- `${CLAUDE_PLUGIN_DATA}` - Plugins数据目录的路径

这允许Plugins包含自定义验证和自动化hooks。

## MCP 工具hooks

MCP 工具遵循模式 `mcp__<server>__<tool>`：
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"systemMessage\": \"Memory operation logged\"}'"
          }
        ]
      }
    ]
  }
}
```
## 安全考虑

### 免责声明

**使用需要您自担风险**：hooks执行任意 shell 命令。您全权负责：
- 您配置的命令
- 文件访问/修改权限
- 潜在的数据丢失或系统损坏
- 在生产使用前在安全环境中测试hooks

### 安全说明

- **需要工作空间信任：** `statusLine` 和 `fileSuggestion` hooks输出命令现在需要接受工作空间信任才能生效。
- **HTTP hooks和环境变量：** HTTP hooks需要显式 `allowedEnvVars` 列表才能在 URL 中使用环境变量插值。这可以防止敏感环境变量意外泄漏到远程端点。
- **托管设置层次结构：** `disableAllHooks` 设置现在遵循托管设置层次结构，这意味着组织级设置可以强制禁用各个用户无法覆盖的hooks禁用。

### 最佳实践

|做|不要|
|-----|--------|
|验证并清理所有输入 |盲目信任输入数据 |
|引用 shell 变量：`"$VAR"` |使用不带引号的：`$VAR` |
|块路径遍历 (`..`) |允许任意路径 |
|使用带有 `$CLAUDE_PROJECT_DIR` | 的绝对路径硬编码路径|
|跳过敏感文件（`.env`、`.git/`、密钥）|处理所有文件 |
|首先单独测试Hook |部署未经测试的Hook |
|对 HTTP hooks使用显式 `allowedEnvVars` |将所有环境变量公开给 webhooks |

## 调试

### 启用调试模式

使用调试标志运行 Claude 以获得详细的Hook日志：
```bash
claude --debug
```
### 详细模式

在 Claude 代码中使用 `Ctrl+O` 启用详细模式并查看Hook执行进度。

### 独立测试Hook
```bash
# Test with sample JSON input
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# Check exit code
echo $?
```
## 完整配置示例
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh\"",
            "timeout": 30
          },
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py\""
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-init.sh\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Verify all tasks are complete before stopping.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```
## Hook执行细节

|方面|行为 |
|--------|----------|
| **超时** |默认 60 秒，每个命令可配置 |
| **并行化** |所有匹配的Hook并行运行|
| **重复数据删除** |相同的Hook命令重复数据删除 |
| **环境** |在 Claude Code 环境的当前目录中运行 |

## 故障排除

### hooks未执行
- 验证 JSON 配置语法是否正确
- 检查匹配器模式是否与工具名称匹配
- 确保脚本存在并且可执行：`chmod +x script.sh`
- 运行 `claude --debug` 查看Hook执行日志
- 验证Hook从标准输入读取 JSON（不是命令参数）

### 钩块意外出现
- 使用示例 JSON 测试hooks：`echo '{"tool_name": "Write", ...}' | ./hook.py`
- 检查退出代码：0 表示允许，2 表示阻止
- 检查 stderr 输出（显示在退出代码 2 上）

### JSON 解析错误
- 始终从标准输入读取，而不是命令参数
- 使用正确的 JSON 解析（而不是字符串操作）
- 优雅地处理缺失字段

## 安装

### 第 1 步：创建 Hooks 目录
```bash
mkdir -p ~/.claude/hooks
```
### 第 2 步：复制示例 Hook
```bash
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```
### 第 3 步：在“设置”中配置
使用上面所示的hooks配置编辑 `~/.claude/settings.json` 或 `.claude/settings.json`。

## 相关概念

- **[Checkpoints and Rewind](../08-checkpoints/)** - 保存和恢复对话状态
- **[Slash Commands](../01-slash-commands/)** - 创建自定义斜线命令
- **[Skills](../03-skills/)** - 可重复使用的自主功能
- **[Subagents](../04-subagents/)** - 委派任务执行
- **[Plugins](../07-plugins/)** - 捆绑的扩展包
- **[Advanced Features](../09-advanced-features/)** - 探索高级 Claude 代码功能

## 其他资源

- **[Official Hooks Documentation](https://code.claude.com/docs/en/hooks)** - 完整的Hook参考
- **[CLI Reference](https://code.claude.com/docs/en/cli-reference)** - 命令行界面文档
- **[Memory Guide](../02-memory/)** - 持久上下文配置
