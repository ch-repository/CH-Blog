---
title: ubuntu系统安装nodev16
date: 2023-05-06 15:22:18
tags: 服务器
---
以下是在Ubuntu系统中使用apt-get安装Node.js v16的步骤：

* 打开终端（Terminal）。
* 更新系统包管理器（PackageManager）：sudo apt-get update。
* 安装基础软件包：sudo apt-get install -y curl dirmngr apt-transport-https lsb-release ca-certificates。
* 添加 Node.js 的 GPG 密钥：curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -。
* 安装 Node.js：sudo apt-get install -y nodejs。
* 确认 Node.js 版本：node -v。
* 确认 NPM 版本：npm -v。

> 请注意，上述步骤假设你正在使用Ubuntu操作系统。其他操作系统可能需要不同的步骤和命令来安装Node.js v16。
