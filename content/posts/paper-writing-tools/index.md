---
author: Nicegeek
title: "论文写作工具链"
description: "介绍我在写论文时常用的工具。"

slug: paper_writing_tools
resources:
- name: "featured-image"
  src: "images/zotero_vscode.png"

tags: ["写作", "工具"]
categories: ["research"]

isCJKLanguage: true
draft: false

weight: 2
date: 2021-06-21T08:09:14+08:00
lastmod: 2021-06-21T08:09:14+08:00
---

主要介绍文献管理Zotero和编辑器Vscode以及在写论文时常用的插件。

<!--more-->

# 文献管理器Zotero
[Zotero下载地址](https://www.zotero.org/download/)

## 使用该文献管理器的原因

- 开源的文献管理器，有强大的社区支持，跨平台，最近推出了IOS版本
- 可以定制化地实现一些自己想要的功能
- 强大的文献收集和管理功能能力
- 可以与vscode、word等编辑软件联动

## 使用技巧
### 文献库和文件同步方式的选择
共有三种同步方式：
- 都利用Zotero软件提供的空间进行同步：方便快捷，但是免费用户的空间较少
- 利用Zotero同步文献库，利用带WebDev功能的网盘同步文献： 如坚果云等，缺点是每个文献都放在一个单独的文件夹，要通过Zotero软件找到文件位置，云端是以压缩包的形式存储，不够灵活
- 利用Zorero同步文献库，文件集中存放在某个文件夹，以附件的方式作为链结添加到每个Zotero条目上，单独使用带同步功能的网盘，如坚果云，OneDrive，google网盘等，将文件夹同步到云端。

建议采用第三种方式，解决了Zotero软件自带的云空间较小的问题，同时也便于文献在云端进行查找和分享。（唯一缺点是IOS版本目前不支持此种方式，有其他替代方案）

实现步骤：

1.安装[Zotfile](https://github.com/jlegewie/zotfile)插件。 ![Github stars](https://img.shields.io/github/stars/jlegewie/zotfile.svg)

2.设置Zotero的附件文件夹，如下图所示：
![](https://pic.rgsc.top/2021-06-21-f167567e-2021-06-21_08-38.png)
这里的设置是确保在不同的机器上使用时，通过相对位置能找到对应的文件。

3.设置Zotfile附件保存位置
![](https://pic.rgsc.top/2021-06-21-cf629919-zotfile.png)

这里第一个设置项目”Source Folder for Attaching New Files“可以设置一个文献下载文件夹的地址，在Zotero中选择”附件新文件“会把该文件夹中**最新的一个文件**，移动到你的Zotero文件夹，并作为链结添加到条目上。

另外这个插件还具备了自动重命名功能，可以根据你自己定义的规则对下载的文件进行重命名。

### 文献搜索和下载
几种文献添加方式：

1.直接用DoI或者arXiv号，添加文件，让Zotero自动下载，可以实现直接从SCI-Hub上下载文献。
实现方法：
设置---Advanced---Config Editor
找到extensions.zotero.findPDFs.resolvers, 写入以下代码：
```
{"name":"Sci-Hub", "method":"GET", "url":"https://sci-hub.st/{doi}", "mode":"html", "selector":"#pdf", "attribute":"src", "automatic":true }
```

2.用浏览器插件，在网页上直接获取文献元信息，让Zotero自动下载。

3.将下载好PDF拖入Zotero，可以自动提区PDF文件自带的文献元信息。

建议采用第一和第二种方式，文献元信息比较完整。第三种方式获取完元信息后，将PDF改为以附件的方式链结到条目上。

### 文献引用

日常要养成好的文献管理习惯，对下载的文献进行分类，维护好文献的元信息，这样才不至于每次引用文献时，要做大量的修改和调整工作。主要讲几个技巧和一些细节：

1.在latex中引用文献，使用[Better Bibtex](https://github.com/retorquere/zotero-better-bibtex)插件：![Github stars](https://img.shields.io/github/stars/retorquere/zotero-better-bibtex.svg)

（结合Vscode的 Zotero Latex插件）
- 可以设置自动导出.Bib文件，
- 管理文献的Citekey，确保唯一不重复
- Pin Citekey确保Citekey不会改变
- 可定制导出的文献元字段
![](https://pic.rgsc.top/2021-06-21-012f903c-2021-06-21_09-30.png)

2.解决IEEE文献引用中的缩写问题
IEEE文献引用通常需要对期刊和会议进行缩写。
- 期刊缩写可以填写在字段“Journal Abbr” 中。

- 会议缩写Zotero没有自带的字段。解决方法：

   - 在“Extra”字段，添加：tex.confAbbr: Conf. name.
   - 在Better Bib设置---Advanced --- postscript 添加代码：
   ```
   if (Translator.BetterBibTeX	&& item.itemType ==='conferencePaper') {
     if (reference.has.confabbr){
      reference.add({ name: 'booktitle', value: reference.has.confabbr.value});  
      reference.remove('confabbr'); 
     }
    }
    ```

3.Word中引用文献
使用Zotero的Word插件，直接插入文献。
- 在设置---Cite中添加文献引用样式（大论文的样式*JM Chinese Std GB/T 7714-2005-AuLower-Bilan*）
- 在设置---Export中使用对应的样式。
![](https://pic.rgsc.top/2021-06-21-a9da48ab-2021-06-21_09-49.png)

## 其他好用的插件
- ![Github stars](https://img.shields.io/github/stars/bwiernik/zotero-shortdoi.svg) [DOI manager](https://github.com/bwiernik/zotero-shortdoi)： 自动从网上获取文献的DOI号
- ![Github stars](https://img.shields.io/github/stars/eschnett/zotero-citationcounts.svg) [Zotero Citation Counts Manager](https://github.com/eschnett/zotero-citationcounts): 获取文献引用量
- ![Github stars](https://img.shields.io/github/stars/dcartertod/zotero-plugins.svg) [ZoteroPreview](https://github.com/dcartertod/zotero-plugins): 添加文献引用预览
- ![Github stars](https://img.shields.io/github/stars/l0o0/jasminum.svg) [Jasminum](https://github.com//l0o0/jasminum)： 用于识别中文元数据
- ![Github stars](https://img.shields.io/github/stars/UB-Mannheim/zotero-ocr.svg) [Zotero OCR](https://github.com/UB-Mannheim/zotero-ocr): OCR 工具
- ![Github stars](https://img.shields.io/github/stars/wshanks/Zutilo.svg) [Zutilo Utility for Zotero](https://github.com/wshanks/Zutilo): 为zotero添加快捷键


# 编辑器软件VsCode
## 写论文常用插件
不同的插件使得VScode的功能非常强大，介绍几个写论文必备的：
- Latex Wokshop: 编译Latex
- Latex language support: 提供latex代码高亮，代码片段
- Zotero Latex： 结合Zotero的Better Bib插入参考文献
- Spell Right： 离线的拼写检查工具
- Grammarly： 在VsCode中使用grammarly进行语法检查
- GitLens： 论文备份和版本管理

## 几个建议
- 建议使用vim编辑方式：
初期适应过程较慢，一旦用熟练了，所有操作不依赖鼠标，效率大大提高。
- 学会设置自己的Code Snippets，通过日常积累代码片段，提升效率
- 利用自动补全和Snippets功能，练习手打公式（IEEE公式编辑参考博文[IEEE期刊投稿公式编辑方法](https://blog.nicegeek.tech/posts/how-to-edit-equation-for-ieee/)）
- 用git备份论文，做好版本管理，也可以备份到github上