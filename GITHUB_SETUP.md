# GitHub 上传指南

## 本地仓库已准备完成

您的本地 git 仓库已经初始化并完成了初始提交。

## 上传到 GitHub 的步骤

### 1. 在 GitHub 上创建新仓库

1. 登录 [GitHub](https://github.com)
2. 点击右上角的 `+` 按钮，选择 `New repository`
3. 填写仓库信息：
   - **Repository name**: `employment` (或您喜欢的名称)
   - **Description**: `Qwen/GLM 模型 OpenAI 兼容 API 服务器`
   - **Public/Private**: 根据需要选择
   - **不要勾选** "Add a README file"、"Add .gitignore"、"Choose a license"（我们已经准备好了）
4. 点击 `Create repository`

### 2. 添加远程仓库并推送

创建仓库后，GitHub 会显示设置命令。请执行以下命令：

```bash
# 添加远程仓库（替换 YOUR_USERNAME 为您的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/employment.git

# 将主分支重命名为 main（推荐）
git branch -M main

# 推送代码到 GitHub
git push -u origin main
```

### 3. 验证上传成功

推送完成后，在浏览器中打开您的仓库地址：
```
https://github.com/YOUR_USERNAME/employment
```

您应该能看到：
- 📄 README.md 的内容
- 📜 LICENSE 文件
- 📁 scripts/ 目录
- 📝 requirements.txt

### 4. （可选）设置 GitHub 仓库描述

在 GitHub 仓库页面：
1. 点击右上角的 ⚙️ Settings
2. 在 "About" 部分添加：
   - **Description**: `Qwen/GLM 模型 OpenAI 兼容 API 服务器`
   - **Website**: （可选）
   - **Topics**: 添加一些标签，如：`llm`, `api-server`, `qwen`, `glm`, `fastapi`, `gradio`

### 5. （可选）添加 GitHub Actions 持续集成

如果需要自动测试，可以创建 `.github/workflows/ci.yml`：

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
```

## 常见问题

### 认证问题

如果推送时遇到认证错误，请使用 GitHub Personal Access Token：

1. 生成 token: Settings → Developer settings → Personal access tokens → Tokens (classic)
2. 选择 `repo` 权限
3. 使用 token 代替密码进行认证

### 推送被拒绝

如果 GitHub 仓库已存在内容，使用强制推送（谨慎使用）：
```bash
git push -f origin main
```

### 查看远程仓库状态

```bash
# 查看远程仓库
git remote -v

# 查看远程分支
git branch -r
```

## 后续操作

推送成功后，您可以：
1. ✏️ 在 README.md 中添加更多项目截图
2. 🏷️ 在 Issues 中添加功能建议
3. 📝 在 Wiki 中添加更多文档
4. 🔖 创建 Releases 发布版本

---

**完成！** 🎉 您的项目已准备好上传到 GitHub。