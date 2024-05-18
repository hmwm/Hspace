---
title: Shadowsocks使用
date: 2024-05-18 20:26:05
tags: "play"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image23.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/image26.jpg
---
如何在本地网络上设置代理服务器并通过 Quantumult X 使用它。这包括配置代理服务器软件并确保 Quantumult X 可以通过本地网络访问它。

### 设置和使用自己的服务器作为代理服务器

### 1. 在本地服务器上安装代理服务器软件

### 安装 Shadowsocks

1. **安装 Shadowsocks 服务端**：
    - 以 Ubuntu 为例，登录到服务器并运行以下命令：
        
        ```bash
        sudo apt update
        sudo apt install shadowsocks-libev
        ```
        
2. **配置 Shadowsocks**：
    - 创建或编辑 Shadowsocks 配置文件：
        
        ```bash
        sudo nano /etc/shadowsocks-libev/config.json
        ```
        
    - 配置文件内容示例如下：
        
        ```json
        {
          "server": "0.0.0.0",
          "server_port": 8388,
          "password": "yourpassword",
          "timeout": 300,
          "method": "aes-256-gcm"
        }
        
        ```
        
    - 保存并退出。
3. **启动 Shadowsocks 服务**：
    - 启动并使其在开机时自动启动：
        
        ```bash
        sudo systemctl start shadowsocks-libev
        sudo systemctl enable shadowsocks-libev
        
        ```
        

### 安装 V2Ray

1. **安装 V2Ray**：
    - 登录到服务器，运行以下命令安装 V2Ray：
        
        ```bash
        bash <(curl -L -s <https://install.direct/go.sh>)
        
        ```
        
2. **配置 V2Ray**：
    - 编辑 V2Ray 配置文件：
        
        ```bash
        sudo nano /etc/v2ray/config.json
        
        ```
        
    - 配置文件内容示例如下：
        
        ```json
        {
          "inbounds": [{
            "port": 443,
            "protocol": "vmess",
            "settings": {
              "clients": [{
                "id": "your-uuid",
                "alterId": 0
              }]
            }
          }],
          "outbounds": [{
            "protocol": "freedom",
            "settings": {}
          }]
        }
        
        ```
        
    - 保存并退出。
3. **启动 V2Ray 服务**：
    - 启动并使其在开机时自动启动：
        
        ```bash
        sudo systemctl start v2ray
        sudo systemctl enable v2ray
        
        ```
        

### 2. 在 Quantumult X 中配置您的代理服务器

1. **打开 Quantumult X 并进入设置界面**：
    - 打开 Quantumult X，点击右下角的齿轮图标进入设置界面。
2. **添加代理服务器**：
    - 在设置界面中，点击“服务器”选项，进入服务器配置页面。
    - 点击右上角的“+”号按钮添加一个新的代理服务器。
3. **输入代理服务器信息**：
    - 根据您设置的服务器信息填写相应的配置。例如，Shadowsocks 配置：
        - **类型**: Shadowsocks
        - **地址**: 您服务器的IP地址或域名
        - **端口**: `8388`
        - **加密方式**: `aes-256-gcm`
        - **密码**: 您设置的密码
    - V2Ray 配置：
        - **类型**: V2Ray
        - **地址**: 您服务器的IP地址或域名
        - **端口**: `443`
        - **UUID**: 您配置的UUID
        - **AlterId**: `0`
        - **加密方式**: `auto`
        - **网络类型**: `tcp`
4. **保存配置**：
    - 输入完所有信息后，点击“保存”或“完成”按钮。
5. **选择并启用代理服务器**：
    - 返回“服务器”页面，点击您刚刚添加的代理服务器，确保它被选中。
    - 返回 Quantumult X 的主界面，开启左上角的代理开关。

通过以上步骤，可以在自己的服务器上设置并配置代理服务器，并在 Quantumult X 中使用它。能确保数据的隐私和安全。