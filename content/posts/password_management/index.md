---
author: Nicegeek
title: "用Pass管理自己的密码"
description: "pass是一个简单用命令行进行个人密码管理的软件，将你的密码用`gpg`加密后，存储在文件夹`~/.password-store`中。"

slug: password_management
resources:
- name: "featured-image"
  src: "images/password.png"

tags: ["gpg key", "secret"]
categories: ["technology"]

isCJKLanguage: true
draft: false

weight: 3
date: 2020-02-19T09:27:56+08:00
lastmod: 2020-03-04T16:29:41+08:00
---

pass是一个简单用命令行进行个人密码管理的软件，将你的密码用`gpg`加密后，存储在文件夹`~/.password-store`中。

<!--more-->

(关于`gpg`的使用可以参考我的另外一篇博客，[如何生成GPG key用于加密传输及Git使用举例](https://www.rgsc.top/ru-he-sheng-cheng-gpg-keyyong-yu-jia-mi-chuan-shu-ji-gitshi-yong-ju-li/))。pass提供了一系列用于操作密码仓库的指令，如添加、删除、编辑、复制到粘贴板等，并可同步至github，也可以自动生成一定长度的密码。只要保管好自己的`gpg`私钥，这个密码管理方案有极高的安全性。虽然不像一些商业软件如`1password`、`bitwarden`等，有比较多的功能，但是pass的优点是：

- 免费
- 完全自己掌控自己的私密信息
- 基本满足日常密码管理和使用的需求

下面介绍一下pass的用法，关于`gpg`密钥生成和管理，可以参照上面提到的博客，本文不再赘述。
[toc]
## 安装pass
pass的安装十分简单，可根据自己使用的系统，用包管理器进行安装
```bash
# Ubuntu/Debian
sudo apt-get install pass

# Arch
pacman -S pass

#MacOS
brew install pass
```

## 使用方法
### 密码的增删查
```bash
## 新建密码库
## --------
pass init nicegeek@example.com # nicegeek@example.com 是你的gpg密钥ID
```
-----
```bash
## 编辑密码库
## --------
# 添加密码
pass insert Email/mail.163.com # 随后输入密码, 这里Email是密码的分类，后面是名称

# 自动生成新密码
pass generate Email/mail.163.com 15 # 生成长度为15，包含字母、数字、符号的密码
pass generate -n Email/mail.163.com 19 # 生成长度为19，包含字母和数字的密码
pass generate -c Email/mail.163.com 12 # 生成密码，并复制到粘贴板

# 删除密码
pass remove Email/mail.163.com
```
还有一些复制，重命名等操作，可以参考pass的[man page](https://git.zx2c4.com/password-store/about/).

----

```bash
## 查找密码
## ------
# 显示密码库目录结构
pass

# 查找匹配的密码名称
pass find .com # 查找所有名称中含有.com的密码

# 显示某个密码的内容
pass Email/mail.163.com

# 复制密码到粘贴板
pass -c Email/mail.163.com
```

### 密码库同步到github
```bash
# 密码库git初始化
pass git init

# 添加远程仓库(在github上先新建好)
pass git remote add origin https://github.com/nicegeeker/my_pass.git

# 推送至远程仓库
pass git push -u --all
```

这些命令行的功能实现，可以结合其他工具，提高使用效率，如MacOS的`Alfred`，Linux系统的`ulauncher`等类似软件，简单几个快捷键，就能找到自己密码，复制到粘贴板，直接使用，感兴趣的可以自己研究一下。