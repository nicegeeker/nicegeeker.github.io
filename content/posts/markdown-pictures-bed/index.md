---
weight: 3
title: "七牛云qshell工具和mac的automator制作永久免费的markdown图床"
date: 2020-02-19T09:27:56+08:00
lastmod: 2020-03-04T16:29:41+08:00
draft: false
description: "利用七牛云的对象存储服务和mac的automator功能，打造一个可以一键上传图片文件，并自动将链接地址保存到粘贴板的功能。"
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["bolg","markdown"]
categories: ["technology"]
---

用markdown写文档时，用图片在本地的相对位置，可以正常显示，但是当需要上传至自己的博客时，图片就看不到了。利用七牛云的对象存储服务和mac的automator功能，打造一个可以一键上传图片文件，并自动将链接地址保存到粘贴板的功能。具体步骤如下：

<!--more-->

## 注册七牛云帐号，并开启一个免费的对象存储服务
[七牛云](https://portal.qiniu.com/signup?code=1h5qh8sbdbpzm)
![七牛云对象存储](http://pic.rgsc.top/2020-12-04-62b8cb8d-Jietu20201204-093200.png)

## 下载七牛云的qshell命令行工具
[qshell下载地址](https://developer.qiniu.com/kodo/tools/1302/qshell#4)
解压缩后，放入文件夹/opt/local/bin/,并赋予可执行权限。

```bash
chmod +x /opt/local/bin/qshell
```
设置qshell账户：
```bash
qshell account accesskey secretkey name
```
accesskey和secretkey可以在七牛云账户“密钥管理”中找到， name自己设置。



## 利用automator配置服务

![配置示例图片](http://pic.rgsc.top/2020-12-04-bef87e47-%3aUsers%3asat%3aPictures%3aclip%3aJietu20201204-093958.png)

如图所示：

- 添加两个命令卡片： “运行shell脚本”和“运行AppleScript”
- 服务设置为：文件或文件夹
- 位于：任何应用程序
- shell:/bin/bash
- 传递输入：作为自变量

### shell脚本：
```bash
urlencode() {
  local length="${#1}"
  for (( i = 0; i < length; i++ )); do
    local c="${1:i:1}"
    case $c in
      [a-zA-Z0-9.~_-]) printf "$c" ;;
    *) printf "$c" | xxd -p -c1 | while read x;do printf "%%%s" "$x";done
  esac
done
}

for f in "$@"

do
    if [ -f $f ]; then
        Key=$(date +%F)-$(date +%s | md5 | head -c 8)-$(basename $f)
        /opt/local/bin/qshell fput nicegeek-md-pic "$Key" $f
        link="http://pic.rgsc.top/$(urlencode $Key)"
        if [ "$links" == "" ]; then
            links=$link
        else
            links=$links"\n"$link
        fi
    fi
done

echo -ne "![]($links)" | pbcopy
```

脚本中要修改两处：
```bash
/opt/local/bin/qshell fput nicegeek-md-pic "$Key" $f
```
这一行中的“nicegeek-md-pic”要修改成自己建的对象云存储仓库的名称。

```bash
link="http://pic.rgsc.top/$(urlencode $Key)"
```
这一行中的“http://pic.rgsc.top/”要修改成对象存储空间绑定的域名。**七牛提供的测试域名1个月就失效了**，可以自己买一个并备案，这个有点麻烦，可以用[*阿里云*](https://wanwang.aliyun.com/?source=5176.11533457&userCode=drns2vxc)注册域名，进行备案。

### Apple Script通知内容
```
display notification "Markdown链接已自动复制" with title "图片上传成功" sound name "default"
```
标题和内容都可以自己定制，声音也可以自己定制，我使用的是默认声音 default, 其他声音可以在 /System/Library/Sounds 这个路径下找到。

将automate文件保存并命名。

## 设置服务快捷键

设置方法：
![](http://pic.rgsc.top/2020-12-04-3030f9e2-Jietu20201204-100232.png)

打开“系统偏好设置” -> “键盘” -> "快捷键" -> "服务" ， 找到刚刚用automator 创建的服务名称，我的名称是 “ 上传到七牛云-md” ， 然后将快捷键设置为 ⌘+U。

## 参考链接
> [利用七牛 qshell 和 Automator 打造快捷上传服务](https://segmentfault.com/a/1190000012625867)
> [自制永久免费的私有图床，替代iPic，你值得拥有](https://www.jianshu.com/p/9572203b6840)