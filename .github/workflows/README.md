# GitHub Actions Workflow 配置说明

## 工作流概述

这个仓库配置了两个GitHub Actions工作流：

1. **HTML项目构建和验证** - 在每次push到master分支时运行
2. **公网预览** - 在PR或手动触发时运行，提供30分钟的临时公网访问

## 公网预览功能配置

### 使用ngrok（推荐）

为了获得稳定的公网访问，建议配置ngrok认证令牌：

1. 注册ngrok账号：https://ngrok.com/
2. 获取authtoken：登录后进入 https://dashboard.ngrok.com/get-started/your-authtoken
3. 在GitHub仓库设置secrets：
   - 进入仓库Settings → Secrets and variables → Actions
   - 点击"New repository secret"
   - Name: `NGROK_AUTH_TOKEN`
   - Value: 你的ngrok authtoken

### 匿名模式（无需配置）

如果没有设置`NGROK_AUTH_TOKEN`，工作流将使用ngrok的匿名模式，但有如下限制：
- 会话时间限制
- 带宽限制
- 每次URL不同

## 如何触发公网预览

### 方法1：手动触发（推荐）
1. 进入GitHub仓库的"Actions"标签页
2. 选择"HTML Project Deployment & Preview"工作流
3. 点击"Run workflow"
4. 选择分支（默认master）
5. 点击"Run workflow"按钮

### 方法2：创建Pull Request
1. 创建新的分支并推送更改
2. 创建Pull Request到master分支
3. 工作流会自动运行并提供预览链接

## 预览功能特点

- **运行时间**：30分钟自动停止
- **访问方式**：通过ngrok提供的公网URL
- **服务器**：Python HTTP服务器（端口8080）
- **自动清理**：工作流结束后自动停止所有服务

## 查看预览URL

工作流运行时，在"Setup ngrok for public access"步骤会输出公网URL，例如：
```
🌍 Public URL: https://abc123.ngrok-free.app
```

你可以在浏览器中打开这个URL访问你的HTML项目。

## 注意事项

1. 预览URL是临时的，每次运行都会不同
2. 30分钟后自动停止，如需延长需要重新运行工作流
3. 如果使用匿名模式，可能会有连接限制
4. 确保index.html文件在项目根目录