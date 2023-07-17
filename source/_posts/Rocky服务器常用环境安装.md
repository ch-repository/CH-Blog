---
title: Rocky服务器常用环境安装
date: 2023-07-17 09:07:10
tags:
  - 服务器
categories: 服务器
comments: false
---

### 必备软件
- nvm
- pm2
- nginx
- docker/docker-compose

#### nvm
> 官方文档：https://github.com/nvm-sh/nvm#install--update-script
安装命令：
```bash
sudo yum update

wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
source ~/.bashrc

// 验证安装
command -v nvm
```

如果nvm install没有反应，请参考这篇文章：https://blog.csdn.net/Lin_madman/article/details/129488898
// 核心代码
```bash
# 这里是你自己的刚刚下好的nvm的路径
export NVM_DIR="~/Documents/nvm/nvm-0.39.3"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
# This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  
# This loads nvm bash_completion
# nodejs下载更换淘宝镜像
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

#### pm2
安装命令：
```bash
npm install pm2 -g
```

#### nginx
安装命令：
```bash
sudo yum install nginx // 安装
sudo systemctl start nginx // 启动Nginx服务
sudo systemctl status nginx // 验证Nginx是否成功安装并正在运行
sudo systemctl enable nginx // 配置Nginx自启动
```

#### docker/docker-compose
安装命令：
```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2 // 安装Docker的依赖项
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo // 添加Docker的软件源
sudo yum install -y docker-ce docker-ce-cli containerd.io // 安装Docker引擎
sudo systemctl start docker // 启动Docker服务
sudo usermod -aG docker $USER // 将当前用户添加到docker用户组（可选，以便免去每次使用Docker时都需要sudo权限）

// docker-compose
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose // 运行命令安装

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose // 运行命令安装 备选方案

chmod +x /usr/local/bin/docker-compose // 给docker-compose执行权限，运行命令
docker-compose version // 查看docker-compose版本
```

### 连接SSH
```bash
ssh-copy-id remote_user@remote_id
```

### 防火墙
```bash
sudo systemctl enable firewalld // firewalld自启动
sudo systemctl start firewalld // 开启firewalld
sudo systemctl status firewalld // 查看firewalld状态
sudo firewall-cmd --get-default-zone // 检查Firewalld默认区域 默认情况下，Firewalld使用的是public区域
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent // 开放80端口
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent // 开放443端口
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent // 开放22端口
sudo firewall-cmd --reload // 重新加载Firewalld配置
```