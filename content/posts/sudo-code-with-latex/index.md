---
author: Nicegeek
title: "用latex写伪代码"
description: "用latex写伪代码"

slug: latex_algorithm 
resources:
- name: "featured-image"
  src: "images/latex.png"

tags: ["latex"]
categories: ["research"]

isCJKLanguage: true
draft: false

weight: 3
date: 2021-06-19T15:34:56+08:00
lastmod: 2021-06-19T15:35:36+08:00  
---

个人推荐使用Algorithm2e在$latex$中编写伪代码。

<!--more-->
```latex
\usepackage[ruled, linesnumbered, vlined]{algorithm2e}
```

### 括号中的参数定义
- ruled: 让标题显示在上面，否则算法标题在下面
- linesnumbered: 让算法中显示行号
- vlined: 显示代码块的竖线

### 基本语法

|代码|含义|
|:---|:---|
|\\; | 在行末加分号并换行 |
|\caption{} | 插入标题 |
|\SetKwData{命令}{显示值} | 设置关键字 |
|\SetKwInOut{命令}{显示值} | 设置关键字 |
|\SetKwFunction{命令}{显示值} | 设置关键字 |
|\for{条件}{循环语句} | for 条件 do|
|\if{条件}{语句} | if | 
|\while{条件}{循环语句} | while|
|\tcc{注释} | /* 注释 */  |
|\tcp{注释} | // 注释 |
| \eif{条件}{肯定语句}{否定语句} | if 条件 then 肯定语句 else 否定语句 end |
| \BlankLine | 空行|
| \leftarrow | 

### 基本格式
1.通常开始时：给出算法的输出输出（或者Data和Result）
2.空一行，列出一些初始化操作
3.算法主体

### 示例
```latex
\begin{algorithm}
    \SetKwData{Left}{left}
    \SetKwData{This}{this}
    \SetKwData{Up}{up} 
    \SetKwFunction{Union}{Union}
    \SetKwFunction{FindCompress}{FindCompress} 
    \SetKwInOut{Input}{input}
    \SetKwInOut{Output}{output} 
    
    \Input{A bitmap $Im$ of size $w\times l$} 
    \Output{A partition of the bitmap} 
    \BlankLine 
    
    \emph{special treatment of the first line}\; 
    \For{$i\leftarrow 2$ \KwTo $l$}
    { 
        \emph{special treatment of the first element of line $i$}\;
        \For{$j\leftarrow 2$ \KwTo $w$}
            {\label{forins} 
            \Left$\leftarrow$ \FindCompress{$Im[i,j-1]$}\; 
            \Up$\leftarrow$ \FindCompress{$Im[i-1,]$}\; 
            \This$\leftarrow$ \FindCompress{$Im[i,j]$}\; 
            \If(\tcp*[h]{O(\Left,\This)==1}){\Left compatible with \This}
            {\label{lt} \lIf{\Left $<$ \This}{\Union{\Left,\This}}
            \lElse{\Union{\This,\Left}} } 
            \If(\tcp*[f]{O(\Up,\This)==1})
            {\Up compatible with \This}
            {\label{ut} \lIf{\Up $<$ \This}{\Union{\Up,\This}} 
            
            \tcp{\This is put under \Up to keep tree as flat as possible}\label{cmt} 
            \lElse{\Union{\This,\Up}}
            \tcp*[h]{\This linked to \Up}\label{lelse} } 
     } 
            
            \lForEach{element $e$ of the line $i$}{\FindCompress{p}} }
            \caption{disjoint decomposition}
    
    \label{algo_disjdecomp} 
    \end{algorithm}
```
![算法示例](https://pic.rgsc.top/2021-06-19-94df7b37-algorithm.png)
