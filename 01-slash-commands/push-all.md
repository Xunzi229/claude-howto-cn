---
描述：暂存所有更改，创建提交，并推送到远程（谨慎使用）
允许的工具：Bash（git add：*），Bash（git状态：*），Bash（git提交：*），Bash（git推送：*），Bash（git diff：*），Bash（git日志：*），Bash（git pull：*）
---

# 提交并推送一切

⚠️ **注意**：暂存所有更改、提交并推送到远程。仅当确信所有更改都属于一起时才使用。

## 工作流程

### 1. 分析变化
并行运行：
- `git status` - 显示修改/添加/删除/未跟踪的文件
- `git diff --stat` - 显示更改统计信息
- `git log -1 --oneline` - 显示消息样式的最近提交

### 2. 安全检查

**❌ 如果检测到则停止并警告：**
- 秘密：`.env*`、`*.key`、`*.pem`、`credentials.json`、`secrets.yaml`、`id_rsa`、`*.p12`、`*.pfx`、`*.cer`
- API 密钥：任何具有实际值的 `*_API_KEY`、`*_SECRET`、`*_TOKEN` 变量（不是占位符，如 `your-api-key`、`xxx`、`placeholder`）
- 大文件：`>10MB`，没有 Git LFS
- 构建工件：`node_modules/`、`dist/`、`build/`、`__pycache__/`、`*.pyc`、`.venv/`
- 临时文件：`.DS_Store`、`thumbs.db`、`*.swp`、`*.tmp`

**API 密钥验证：**
检查修改后的文件是否存在以下模式：
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ Real key detected!
AWS_SECRET_KEY=AKIA...         # ❌ Real key detected!
STRIPE_API_KEY=sk_live_...    # ❌ Real key detected!

# ✅ Acceptable placeholders:
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```
**✅ 验证：**
- `.gitignore` 正确配置
- 没有合并冲突
- 正确的分支（如果是主分支则发出警告）
- API 密钥只是占位符

### 3.请求确认

目前总结：
```
📊 Changes Summary:
- X files modified, Y added, Z deleted
- Total: +AAA insertions, -BBB deletions

🔒 Safety: ✅ No secrets | ✅ No large files | ⚠️ [warnings]
🌿 Branch: [name] → origin/[name]

I will: git add . → commit → push

Type 'yes' to proceed or 'no' to cancel.
```
**在继续之前等待明确的“是”。**

### 4.执行（确认后）

按顺序运行：
```bash
git add .
git status  # Verify staging
```
### 5. 生成提交消息

分析更改并创建常规提交：

**格式：**
```
[type]: Brief summary (max 72 characters)

- Key change 1
- Key change 2
- Key change 3
```
**类型：** `feat`、`fix`、`docs`、`style`、`refactor`、`test`、`chore`、`perf`、`build`、`ci`

**示例：**
```
docs: Update concept README files with comprehensive documentation

- Add architecture diagrams and tables
- Include practical examples
- Expand best practices sections
```
### 6. 提交并推送
```bash
git commit -m "$(cat <<'EOF'
[Generated commit message]
EOF
)"
git push  # If fails: git pull --rebase && git push
git log -1 --oneline --decorate  # Verify
```
### 7.确认成功
```
✅ Successfully pushed to remote!

Commit: [hash] [message]
Branch: [branch] → origin/[branch]
Files changed: X (+insertions, -deletions)
```
## 错误处理

- **git add 失败**：检查权限、锁定文件、验证存储库已初始化
- **git 提交失败**：修复预提交hooks，检查 git 配置（user.name/email）
- **git推送失败**：
  - 非快进：`git pull --rebase && git push`
  - 无远程分支：`git push -u origin [branch]`
  - 受保护的分支：使用 PR 工作流程代替

## 何时使用

✅ **好：**
- 多文件文档更新
- 带有测试和文档的功能
- 跨文件的错误修复
- 项目范围的格式化/重构
- 配置变更

❌ **避免：**
- 不确定正在做什么
- 包含秘密/敏感数据
- 受保护的分支机构无需审查
- 存在合并冲突
- 想要详细的提交历史记录
- 预提交hooks失败

## 替代方案

如果用户想要控制，建议：
1. **选择性暂存**：审查/暂存特定文件
2. **交互式分期**：`git add -p` 用于补丁选择
3. **PR工作流程**：创建分支→推送→PR（使用`/pr`命令）

**⚠️记住**：在推送之前一定要检查更改。如有疑问，请使用单独的 git 命令进行更多控制。
