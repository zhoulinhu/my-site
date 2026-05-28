---
title: GitHub配置ssh密钥
date: 2026-05-28 17:22:00 +0800
author: zlh
categories: [技术, 运维]
tags: [Jekyll, 博客, ssh密钥]
description: 这是一篇介绍github配置公钥，便于本地git上传的文章
image: false
math: false
mermaid: false
pin: false
---

# SSH密钥准备
> 检查`~/.ssh`文件下是否有公私钥文件，一般类似`id_ed25519、id_ed25519.pub、id_rsa、id_rsa.pub`

## 生成新SSH密钥对(推荐ed25519)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

- -t ed25519: 使用更安全高效的ed25519算法
- -C : 后面填写备注标识，最好填写github邮箱

完成后会生成两个文件在`~/.ssh/id_ed25519和~/.ssh/id_ed25519.pub`

# GitHub添加SSH公钥
> 将上述的`~/.ssh/id_ed25519.pub`文件中的公钥内容复制添加到Github中，下述二选一即可

GitHub中可以账户直接添加公钥：
- 登录GitHub账号--> Settings
- 选择左侧菜单的**SSH and GPG keys**
- 点击**New SSH Key按钮**
- **Title:** 任意名称，标记这台机器
- **Key type:** 选择**Authentication Key**或**Signing Key**
- **Key:** 黏贴刚刚复制的公钥
- 点击**Add SSH key**

GitHub中还有直接给仓库添加公钥:
- 选中要操作的仓库(假如这里是my-site)，然后选择仓库的**Settings**
- 选择左侧菜单的**Deploy keys**
- 点击**Add deploy key**
- **Title:** 任意名称，标记这台机器
- **Key:** 黏贴刚刚复制的公钥
- 选中**Allow write access**复选框，然后点击**Add key**

## 测试ssh连接
```bash
ssh -T git@github.com
```

首次连接会提示：
```text
The authenticity of host 'github.com (IP)' can't be established.
ED25519 key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入`yes`回车。如果看到：
```text
Hi 你的用户名! You've successfully authenticated, but GitHub does not provide shell access.
```

说明配置成功

# 本地git拉取/上传github仓库

现在可以使用SSH协议操作仓库，例如：
```bash
git clone git@github.com:用户名/仓库名.git
```

若已有仓库使用HTTPS地址，可切换为SSH：
```bash
git clone https://github.com/zhoulinhu/my-site.git
# 使用上述拉取的仓库就是HTTP的，可使用下述命令查看是否是http拉取的
git remote -v

# 下述命令就是把仓库切换成SSH的
git remote set-url origin git@github.com:用户名/仓库名.git
```
