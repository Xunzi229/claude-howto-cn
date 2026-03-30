---
名称：设置 CI/CD 管道
描述：实施预提交hooks和 GitHub Actions 以保证质量
标签： CI-CD、DevOps、自动化
---

# 设置 CI/CD 管道

实施适合项目类型的全面 DevOps 质量关卡：

1. **分析项目**：检测语言、框架、构建系统和现有工具
2. **使用特定于语言的工具配置预提交hooks**：
   - 格式：Prettier/Black/gofmt/rustfmt/等。
   - Linting：ESLint/Ruff/golangci-lint/Clippy/等。
   - 安全：Bandit/gosec/cargo-audit/npm 审计/等。
   - 类型检查：TypeScript/mypy/flow（如果适用）
   - 测试：运行相关测试套件
3. **创建 GitHub Actions 工作流程** (.github/workflows/)：
   - 镜像推送/PR 上的预提交检查
   - 多版本/平台矩阵（如果适用）
   - 构建和测试验证
   - 部署步骤（如果需要）
4. **验证管道**：本地测试，创建测试 PR，确认所有检查通过

使用免费/开源工具。尊重现有配置。保持快速执行。
