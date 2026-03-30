<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# DevOps 自动化Plugins

用于部署、监控和事件响应的完整 DevOps 自动化。

## 特点

✅ 自动化部署
✅ 回滚程序
✅ 系统健康监控
✅ 事件响应工作流程
✅ Kubernetes 集成

## 安装
```bash
/plugin install devops-automation
```
## 包含什么

### 斜线命令
- `/deploy` - 部署到生产或登台
- `/rollback` - 回滚到以前的版本
- `/status` - 检查系统健康状况
- `/incident` - 处理生产事件

### Subagents
- `deployment-specialist` - 部署操作
- `incident-commander` - 事件协调
- `alert-analyzer` - 系统健康分析

### MCP 服务器
- Kubernetes集成

### 脚本
- `deploy.sh` - 部署自动化
- `rollback.sh` - 回滚自动化
- `health-check.sh` - 健康检查实用程序

### hooks
- `pre-deploy.js` - 部署前验证
- `post-deploy.js` - 部署后任务

## 用法

### 部署到暂存
```
/deploy staging
```
### 部署到生产环境
```
/deploy production
```
＃＃＃ 回滚
```
/rollback production
```
### 检查状态
```
/status
```
### 处理事件
```
/incident
```
## 要求

- claude代码 1.0+
- Kubernetes CLI (kubectl)
- 配置集群访问

## 配置

设置您的 Kubernetes 配置：
```bash
export KUBECONFIG=~/.kube/config
```
## 工作流程示例
```
User: /deploy production

Claude:
1. Runs pre-deploy hook (validates kubectl, cluster connection)
2. Delegates to deployment-specialist subagent
3. Runs deploy.sh script
4. Monitors deployment progress via Kubernetes MCP
5. Runs post-deploy hook (waits for pods, smoke tests)
6. Provides deployment summary

Result:
✅ Deployment complete
📦 Version: v2.1.0
🚀 Pods: 3/3 ready
⏱️  Time: 2m 34s
```
