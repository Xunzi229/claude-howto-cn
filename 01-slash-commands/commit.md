---
允许的工具：Bash(git add:*)、Bash(git status:*)、Bash(git commit:*)、Bash(git diff:*)
参数提示：[消息]
描述：创建带有上下文的 git 提交
---

## 上下文

- 当前 git 状态：!`git status`
- 当前 git diff：!`git diff HEAD`
- 当前分支：!`git branch --show-current`
- 最近提交：!`git log --oneline -10`

## 你的任务

基于上述更改，创建单个 git 提交。

如果消息是通过参数提供的，请使用它：$ARGUMENTS

否则，分析更改并按照常规提交格式创建适当的提交消息：
- `feat:` 新功能
- `fix:` 用于错误修复
- `docs:` 用于文档更改
- `refactor:` 用于代码重构
- `test:` 用于添加测试
- `chore:` 用于维护任务
