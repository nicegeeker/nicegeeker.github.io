---
weight: 3
title: "如何生成GPG key用于加密传输及Git使用举例"
date: 2020-02-19T09:27:56+08:00
lastmod: 2020-03-04T16:29:41+08:00
draft: false
description: "GPG：GNU Privacy Guard，a great password generator. GPG允许您加密和解密信息。它基于使用一对密钥，一个公钥和一个私有密钥。使用一个密钥加密的数据只能用另一个密钥解密。

"
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["gpg key", "secret"]
categories: ["technology"]
---


GPG：GNU Privacy Guard，a great password generator

GPG应用程序允许您加密和解密信息。它基于使用一对密钥，一个公钥和一个私有密钥。使用一个密钥加密的数据只能用另一个密钥解密。

*   要加密发送给你的邮件，可使用你的公钥创建，只能使用你的私钥解锁。  
    ![](http://pic.rgsc.top/2020-12-07-6f3dddc6-gpg.jpeg)
*   要对信息进行签名，可以使用私钥锁定信息，允许任何人使用公钥解锁信息来验证信息是否来自你。
*   也可用自己的私钥和对方的公钥加密，同时实现加密传输和数字签名。  
    ![](http://pic.rgsc.top/2020-12-07-3d278821-gpg-en-sign.jpeg)

公钥可以发送给任何人，而私钥只能由你自己保管。这些都可以通过`gpg`用命令行的方式实现。

## 软件安装

直接使用相应系统的包管理器安装`gnupg`即可。
```bash
    # Debian/Ubuntu
    sudo apt-get install gnupg

    # Arch Linux
    sudo pacman -S gnupg

    # MacOS
    brew install gnupg
```

## 建立和管理GPG密钥对

### 生成密钥对

    gpg --full-generate-key

*   选择建立 RSA/RSA(1)
*   用4096 bits的密钥提高安全性
*   设置一个过期日期，可以设置为一年(1y)，万一遭遇_社会工程学_攻击，电脑被盗，密钥丢失，可以一定程度上降低损失。
*   设置一个容易记住的密码

### 一些常用的操作命令

    # 列出本机上的密钥
    gpg --list-keys

    # 更改gpg密钥的密码, "nicegeek@example.com" 改成你自己密钥的邮箱
    gpg --passwd nicegeek@example.com

    # 更改过期时间
    gpg --edit-key nicegeek@example.com

    >gpg key0  # 选择密钥
    >gpg expire # 输入一个新的过期时间
    >gpg save

    # 导出你的GPG公钥
    gpg --export --armor nicegeek@example.com # 直接导出至终端屏幕，可复制
    gpg --export --armor --output nicegeek.gpg.put nicegeek@example.com   # 导出为一个文件

## 密钥备份

### 方法一：

可以将整个~/.gnugp 文件夹进行备份，里面包含了所有的gpg密钥，以及一些设置

    tar czf gnugp.tar.gz ~/.gnugp

保存压缩包`gnugp.tar.gz`到一个比较安全的地方。

### 方法二：

仅备份特定密钥对。

    # 导出的文件中既包含公钥也包含私钥
    gpg --export-secret-keys --armor --output nicegeek.gpg nicegeek@example.com 

    # 导入方法
    gpg --import nicegeek.gpg
    gpg --edit-key nicegeek@example.com trust quit # 信任该密钥，选择5，y

## 使用示例

### 步骤详解

当你的project需要上传至github时，而其中包含的一些私密信息不想泄露，可以建立一个私密文件夹`./secrets`, 将私密信息保存至文件夹中，打包后用GPG加密。  
具体步骤：

1.  将文件移动到私密文件夹，在原来位置设置软链结(假设源文件在`/originfolder/alikey.secret`)

    mv ./originfolder/alikey.secret ./secrets/
    ln -sf ./secrets/alikey.secret ./originfolder/alikey.secret

1.  在`.gitignore`中，将私密文件夹排除在外。

    # .gitngnore 内容
    secrets/

1.  压缩`secrets`文件夹

    tar czf secrets.tar.gz ./secrets

1.  用GPG加密，并删除原压缩文件

    gpg -er nicegeek@example.com secrets.tar.gz
    rm secrets.tar.gz

1.  在其他位置使用时，将git仓库拉到本地，只需解密并解压即可

    gpg -do secrets.tar.gz secrets.tar.gz.gpg
    tar xvf secrets.tar.gz
    rm secrets.tar.gz

### 自动化上述过程

将操作以函数的形式，存入 `~/.zshrc` 或者 `~/.bashrc`

加密函数：

    ensec()
    {
        tar czf secrets.tar.gz ./secrets
        gpg -er nicegeek@example.com secrets.tar.gz
        rm secrets.tar.gz
    }

解密函数：

    desec()
    {
        gpg -do secrets.tar.gz secrets.tar.gz.gpg
        tar xvf secrets.tar.gz
        rm secrets.tar.gz
    }