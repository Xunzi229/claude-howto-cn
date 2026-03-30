---
描述：清理代码、阶段更改并准备拉取请求
允许的工具：Bash(git add:*)、Bash(git status:*)、Bash(git diff:*)、Bash(npm test:*)、Bash(npm run lint:*)
---

# 拉取请求准备清单

在创建 PR 之前，请执行以下步骤：

1. 运行 linting：`prettier --write .`
2. 运行测试：`npm test`
3. 查看 git diff：`git diff HEAD`
4. 阶段变更：`git add .`
5. 在常规提交之后创建提交消息：
   - `fix:` 用于错误修复
   - `feat:` 新功能
   - `docs:` 用于文档
   - `refactor:` 用于代码重组
   - `test:` 用于测试添加
   - `chore:` 用于维护

6. 生成 PR 摘要，包括：
   - 发生了什么变化
   - 为什么改变
   - 进行测试
   - 潜在影响
