---
title: 将hexo部署到自己的服务器上
date: 2023-05-06 15:09:13
tags:
    - 服务器
    - hexo
---

## 第一步（服务端的操作）
### 安装git和nginx
```bash
sudo apt-get install -y nginx git
```

### 添加一个git新用户 
```bash
#添加用户
useradd git
#设置密码
passwd git

# 给git用户配置sudo权限
chmod 740 /etc/sudoers
#编辑sudoers配置文件
vim /etc/sudoers
# 找到root ALL=(ALL) ALL，在它下方加入一行 一般在文件文件最下方
git ALL=(ALL) ALL

chmod 400 /etc/sudoers
```
> 我目前这个博客直接用的root用户

### 给git用户添加ssh密匙
```bash
su - git
mkdir -p ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorzied_keys
chmod 700 ~/.ssh
vim ~/.ssh/authorized_keys    #将ssh密钥粘贴进去
```

### 创建git仓库实现自动部署
创建git仓库并使用git-hooks实现自动部署
```bash
sudo mkdir -p /var/repo    #新建目录，这是git仓库的位置
sudo mkdir -p /var/www/hexo
cd /var/repo  #转到git仓库的文件夹
sudo git init --bare blog.git #创建一个名叫blog的仓库
sudo vim /var/repo/blog.git/hooks/post-update
```
post-update的内如如下：
```bash
#!/bin/bash
git --work-tree=/var/www/hexo --git-dir=/var/repo/blog.git checkout -f
```
### 给post-update授权
```bash
cd /var/repo/blog.git/hooks/
sudo chown -R git:git /var/repo/
sudo chown -R git:git /var/www/hexo
sudo chmod +x post-update  #赋予其可执行权限
```

### 配置nginx
```bash
cd /etc/nginx/conf.d/
vim blog.conf
```
blog.conf的内如如下：
```
server {
    listen    80 default_server;
    listen    [::] default_server;
    server_name    xybin.top;#可以写自己的域名
    root    /var/www/hexo;
}
```
### 检查Nginx语法并重载nginx：
```bash
nginx -t

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

nginx -s reload
```

### 修改git用户的默认shell环境
```bash
vim /etc/passwd
#修改最后一行
#将/bin/bash修改为/usr/bin/git-shell

git:x:1002:1002::/home/git:/usr/bin/git-shell
```

## 第二部（客户端的操作）
> 需要先安装git和nodejs

### 全局安装hexo
```bash
npm install -g hexo-cli
```

### 初始化hexo
```bash
hexo init myblog
```

进入这个文件夹，执行：
```bash
npm install
```

本地查看：
```bash
hexo g
hexo server
```

### 将hexo部署到服务器
安装完hexo就可以将hexo生成的文章部署到服务器上，打开站点配置文件 _config.yml，翻到最后，修改为
```yml
deploy:
  type: git
  repo: git@***.***.***.***:/var/repo/blog.git # IP填写自己服务器的IP即可
  branch: master
```
先安装deploy-git ，才能用命令部署到Git。
```bash
npm install hexo-deployer-git --save
```
然后依次执行以下命令：
```bash
hexo clean
hexo generate
hexo deploy
```
> hexo clean清除了你之前生成的东西，也可以不加。
 hexo generate 生成静态文章，可以用 hexo g缩写
 hexo deploy 部署文章，可以用hexo d缩写

然后输入服务端密码：部署完成！