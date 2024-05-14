---
title: My first blog demo
top_img: "./img/02.jpg"
cover: "./img/02.jpg"
---


# hexo远程自动部署

使用 GitHub 部署 Hexo 网站并不是专门依赖某个特定的平台，而是利用 GitHub 作为源代码和内容管理的中心。你可以通过 GitHub 将生成的静态文件推送到你自己的云服务器上进行部署。以下是如何使用 GitHub 和你自己的云服务器来部署 Hexo 网站的流程：

### 1. **准备 GitHub 仓库**

- 在 GitHub 上创建一个新的仓库，将其作为 Hexo 项目的版本控制仓库。
- 使用 Git 命令行工具或 GUI 工具，将本地的 Hexo 项目推送到这个仓库中。

### 2. **在云服务器上配置 Git**

- 确保你在云服务器上安装并配置了 Git。
- 将云服务器的 SSH 公钥添加到 GitHub 仓库的 "Deploy Keys" 或者你的 GitHub 账号的 "SSH and GPG keys" 中。

### 3. **在云服务器上设置部署脚本**

- 创建一个用于克隆仓库并同步静态文件的脚本。你可以通过 SSH 克隆或拉取 GitHub 上的仓库内容。
- 例如，编写一个简单的 Bash 脚本：
    
    ```bash
    #!/bin/bash
    cd /path/to/website  # 替换为实际路径
    git pull origin main  # 从 GitHub 仓库拉取最新的内容
    hexo generate  # 重新生成静态文件
    
    ```
    

### 4. **Web 服务器设置**

- 确保你的 Web 服务器（如 Nginx 或 Apache）已经正确设置为指向 Hexo 项目生成的 `public` 目录来提供服务。

### 5. **触发部署**

- 使用 GitHub Actions、GitLab CI/CD，或手动触发脚本来将最新内容推送到服务器。
- 可以设置一个自动化的部署流程，例如使用 Webhook，将 GitHub 仓库的推送事件通知到你的服务器，让服务器自动执行部署脚本来拉取和更新网站内容。

这样一来，GitHub 就成为你 Hexo 项目的源代码和内容管理中心，而你可以在自己的云服务器上部署生成的静态文件，实现更灵活的自定义部署。
