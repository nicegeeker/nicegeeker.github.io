---
weight: 3
title: "IEEE期刊投稿公式编辑方法"
date: 2021-06-19T10:42:17+08:00
lastmod: 2021-06-19T10:42:17+08:00
draft: false
description: "向IEEE投稿时，怎么优雅地编辑公式。"
resources:
- name: "featured-image"
  src: "featured-image.png"
tags: ["latex", "公式"]
categories: ["research"]
---
本文介绍了向IEEE期刊投稿时，怎么优雅地编辑公式。

<!--more-->

## 公式编辑环境推荐
1. 在编辑公式前，必须载入公式的基础包`\userpackage{amsmath}`

2. 为了保持全文风格的一致性，建议只使用`IEEEeqnarray`环境，原因：

     - 不同的公式环境，前后留白的大小不一样
     - `equation`环境可以实现自动换行，但是有些时候，公式后面还有公式标号的空间时，也会自动换行后再生成公式标号，而`IEEEequarray`可以完全自由地控制换行的位置
     - `align`环境在某行公式过长时，会产生对齐问题，如下图所示。
     ![`align`环境对齐问题](http://pic.rgsc.top/2020-12-10-f4f8aa47-align_prob.png)
     - `equarray`环境可以解决公式对齐的问题，但是对齐后会导致`=`前后的空格变大，与其他任何一种环境都不一样，所以应**严格不使用该环境**
  ![](http://pic.rgsc.top/2020-12-10-d53659a0-equarray.png)
      - `IEEEeqnarray`环境的灵活性和一致性，可以实现所有公式编辑中需要的功能
  
3. 在使用`IEEEeqnarray`环境时，如果文档使用的是`\documentclass[]{IEEEtran}`，则直接使用即可，如果不是则要载入包`\usepackage{IEEEtrantools}`(回复审稿意见时可能会用到)。

## 基础用法
### 单行公式写法
单行公式，直接一列，用`{c}`居中即可
```latex
\begin{IEEEeqnarray}{c}
  a = b + c
\end{IEEEeqnarray}
```
### 多行公式写法
公式换行准则：
  - 通常在等号或者运算符前进行换行
   - 等号前优先于运算符前换行
   - 加号或者减号前优先于其他运算符前换行
   - 尽量避免其他换行方式
   
多行公式，分成三列，按照`=`进行对齐，左列靠右，中列剧中，右列靠左
```latex
\begin{IEEEeqnarray}{rCl}
  a & = & b + c    \\
    & = & e + f+ g \\
    & = & h + j
\end{IEEEeqnarray}
```
当公式后有括号说明时，可以添加空格列和文字对齐列，如：
```latex
\begin{IEEEeqnarray}{rCl"s}
 ...
\end{IEEEeqnarray}
```
列位置控制符:
| 符号 | 表示 |
| --- | --- |
| l c r L C R | 分别表示数字的靠左，居中，靠右|
| s  t  u| 分别表示文字模式的靠左，居中，靠右 |
| . / ?  "  | 空格大小，逐渐变大|

### 矩阵写法
用`matrix`环境实现：
- `matrix`:无括号的矩阵,
- `pmatrix`:(),
- `bmatrix`:[], 
- `Bmatrix`:{}, 
- `vmatrix`: | |,
- `Vmatrix`: ||  ||

### 控制编号
公式的某行是否编号以及是否有子编号，通过在行内加入 `\IEEEyesnumber`、`\IEEEnonumber`、`\IEEEyessubnumber`和`\IEEEnosubnumber`来实现，上述命令后面加`*`，表示从此行起，遇到新的编号命令前，按照此行的命令执行。如`\IEEEyesnumber*`表示从此行起，后面的每行公式都编号。

编号命令放在每行的最后面，换行符`\\`前。

## 特殊情况及技巧
### 1. 公式某行过长与编号重叠，且公式前还有空间时
这种情况，不需要手动调整将比较长的那一行进行换行，只需在与编号重叠的那一行最后添加`\IEEEeqnarraynumspace`,公式整体会左移，空出公式编号的位置。

*使用此方法时，要注意不要让公式的左侧超出文字的边界。*

### 2. 公式的第一行过长时
可以用`\IEEEeqnarraymulticol{3}{l}{}`将第一行的三列合并为一行，如下式：
```latex
\begin{IEEEeqnarray}{rCl}
  \IEEEeqnarraymulticol{3}{l}{ a + b + c + d + e + f + g + h}\IEEEnonumber\\* 
  \quad & = & i + j \\ 
       & = & k + l + m 
\end{IEEEeqnarray}
```
显示效果如下：
![](http://pic.rgsc.top/2020-12-10-b49fe48b-multi_col.png)
通过，添加`\qquad`可以调整下几行公式缩进的程度，*需特别注意：`\IEEEeqnarraymuticol`前面不可以添加`\lable{}`*。

### 3.控制公式中括号的大小
latex可以自动根据括号中公式的大小，调整对应的括号的大小，只需要在括号前加入`\left`、`\right`或者`\middle`,如以下代码所示：
```latex
\begin{IEEEeqnarray}{c}
    H\left(X \, \middle| \, \frac{Y}{X} \right) 
\end{IEEEeqnarray}
```
显示效果如下：
![](http://pic.rgsc.top/2020-12-11-7ca0acc6-presences.png)

但是还需要解决以下两个问题：

- 用括号表示函数$f()$时，$f$与$()$的距离太远，如下图所示：
   ![](http://pic.rgsc.top/2020-12-11-2725110a-too_far.png)
   可以在文件头加入以下代码解决此问题：
```latex
\usepackage{mleftright}
\mleftright
```
- 当左、右括号之间出现换行时，如下图所示
    ![](http://pic.rgsc.top/2020-12-11-bd6afbb7-swicth_line.png)
    可以在头文件加入以下代码解决此问题：
```latex
\newcommand{\sizecorr}[1]{\makebox[0cm]{\phantom{$\displaystyle #1$}}}
```
    
### 4.防止同一公式的多行分页显示
只需在公式换行符号`\\`后添加`*`

### 5. 控制公式是否能换页
在文件头添加`\interdisplaylinepenalty=xx`。
- 若xx=0，则允许公式换页
 - 若xx=2500, 则由latex自行判断，在没有更好的处理办法时，再进行换页
 - 若xx=10000, 则严格禁止公式换页

### 6.给公式引用添加超链结
只需在文件头添加`\usepackage[colorlinks=true,linkcolor=blue]{hyperref}`

## 几个细节
### 注意区分一元操作符和二元操作符
- 如`-`作为一元操作符是负号，作为二元操作符是减号，当他在公式某行的最前面时，会自动被当作一元操作符，会导致其与后一个符号的距离太近，应在符号后面添加`\>`，使其保持与后侧符号距离，作为二元操作符显示。
- 某些情况下，latex会将一元操作符，显示为二元操作符，如在`\log`和`\sin`前时，为了使其作为一元操作符显示应将后面的表达式用`{}`括起来，如`-{\log a}`。

### 公式的`\lable{}`最好放置在指定公式的最后
公式的`\lable{}`最好放在本行公式的最后，换行符`\\`前，以避免类似`\IEEEeqnarraymulticol`的情况
### 应注意公式里面文字部分的字体
文字部分的字体应与公式符号的字体不一样，应该将文字部分用`\text{}`括起来，或者在指定环境中各列对齐方式时，使用`s, t, u`指定该部分为文字，在文字位置需要添加公式则用行内公式的写法`$...$`实现。

### 正确实现微分符号`d`
在文件头添加`\newcommand{\dd}{\mathop{}\!\mathrm{d}}`，用`\dd`表示微分符号`d`。

### `argmin`、`argmax`及`trace`实现方法
在文件头加入以下代码：
```latex
% argmin argmax  trace
\DeclareMathOperator*{\argminA}{arg\,min}
\DeclareMathOperator*{\argminB}{argmin}
\DeclareMathOperator*{\argmaxA}{arg\,max}
\DeclareMathOperator*{\argmaxB}{argmax}

\DeclareMathOperator{\tr}{Tr}
```
A和B两种样式都可以使用，选择自己习惯的样式。

### 解决不可见的公式引用问题
不可见引用是指，在公式中使用了子编号如`(3a), (3b)`, 并没有显式的出现编号`(3)`， 而文中却引用了公式(3)。此时公式(3)的引用超链结会出现问题。需要在文件头中加入以下代码：
```latex
\makeatletter
\def\IEEElabelanchoreqn#1{\bgroup
\def\@currentlabel{\p@equation\theequation}\relax
\def\@currentHref{\@IEEEtheHrefequation}\label{#1}\relax
\Hy@raisedlink{\hyper@anchorstart{\@currentHref}}\relax
\Hy@raisedlink{\hyper@anchorend}\egroup}
\makeatother
\newcommand{\subnumberinglabel}[1]{\IEEEyesnumber
  \IEEEyessubnumber*\IEEElabelanchoreqn{#1}}
```

### 解决`IEEEeqnarray`中编号字体随环境定义变化的问题
在`IEEEeqnarray`环境中，如果定义了环境的字体，则公式的编号字体也会随之变化，为了避免这个问题，可以在文件头中加入下列代码：
```latex
\renewcommand{\theequationdis}{{\normalfont (\theequation)}} 
\renewcommand{\theIEEEsubequationdis}{{\normalfont (\theIEEEsubequation)}}
```

## 使用进阶
### 1.两组相邻公式之间有简短文字相隔，如何将两组公式进行对齐
具体实现方法如下：
```latex
We have 
\begin{IEEEeqnarray}{rCl}
  \subnumberinglabel{eq:block3} a & = & b + c \label{eq:block3_eq1}\\ 
  & = & d + e \label{eq:block3_eq2}\\ 
  \noalign{\noindent and \vspace{2\jot}} 
 f & = & g - h + i \label{eq:block3_eq3} 
\end{IEEEeqnarray}
```
显示效果如下：
![](http://pic.rgsc.top/2020-12-10-cb2793f2-align.png)

注意中间的and部分实现代码是`\noalign{\noindent and \vspace{2\jot}}`。

### 2. 用`IEEEeqnarraybox`结合`IEEEeqnarraystrutmode`实现复杂的公式结构
`IEEEeqnarraybox`用法与`IEEEeqnarray`类似，其特点是可以嵌套在其他环境中。 在`IEEEeqnarraybox`的属性中添加`IEEEequarraystrutmode`，具体用法见下列示例。
举例1:
```latex
\begin{IEEEeqnarray}{c}
 \IEEEnonumber*
    \begin{IEEEeqnarraybox}[ \IEEEeqnarraystrutmode \IEEEeqnarraystrutsizeadd{3pt} {1pt} ]{c’c/v/c’c’c}
        D_1 & D_2 & & X_1 & X_2 & X_3 \\
        \hline 
        0 & 0 && +1 & +1 & +1\\ 
        0 & 1 && +1 & -1 & -1\\ 
        1 & 0 && -1 & +1 & -1\\ 1 & 1 && -1 & -1 & +1 
    \end{IEEEeqnarraybox} 
\end{IEEEeqnarray}
```
其中`{3pt}`和`{1pt}`表示行前，行后添加的空白大小。

举例2:
```latex
\begin{IEEEeqnarray}{c} 
    \left.
    \begin{IEEEeqnarraybox}[\IEEEeqnarraystrutmode \IEEEeqnarraystrutsizeadd{2pt}{2pt}][c]{rCl} 
    x & = & a + b\\ 
    y & = & a - b 
    \end{IEEEeqnarraybox} 
    \, \right\} \iff \left\{ \, 
    \begin{IEEEeqnarraybox}[ \IEEEeqnarraystrutmode \IEEEeqnarraystrutsizeadd{7pt} {7pt}][c]{rCl} 
    a & = & \frac{x}{2} + \frac{y}{2} \\ 
    b & = & \frac{x}{2} - \frac{y}{2} 
    \end{IEEEeqnarraybox} 
    \right.
    \label{eq:example_left_right2} 
\end{IEEEeqnarray}
```

### 3. 实现函数组中每个函数逐个编号
上一个方法可以实现函数组，但他将函数组作为整体进行编号，要为组中每个函数逐个编号时，采用下述方法：
```latex
\begin{IEEEeqnarray}{rrCl}
    & \dot{x} & = & f(x,u) \\* 
    \smash{\left\{ \IEEEstrut[8\jot] \right.} & x+\dot{x} & = & h(x) \\* 
    & x+\ddot{x} & = & g(x) 
\end{IEEEeqnarray}
```
在左侧再添加一列，用`\smash`实现大括号${$, 其中的`[8\jot]`需要根据公式的大小进行调整，数字为 $(lines \times 2 + 2)$(公式中有`\frac`时是2行)。

- 若公式行数为偶数时，用隐藏行实现大括号的对齐
```latex
\begin{IEEEeqnarray}{rrCl} 
    & \dot{x} & = & f(x,u) \\*
    [-0.625\normalbaselineskip] 
    % start invisible row 
    \smash{\left\{ \IEEEstrut[6\jot] \right.} \nonumber 
    % end invisible row \\*
    [-0.625\normalbaselineskip] 
    & x+\dot{x} & = & h(x) 
\end{IEEEeqnarray}
```
- 若某些公式占用多行导致大括号位置不对称时
```latex
\begin{IEEEeqnarray}{rrCl}
    \subnumberinglabel{eq:uneven2} 
    & a_1 + a_2 & = & \sum_{k=1}^{\frac{M}{2}} f_k(x,u) \\*
    [-0.1\normalbaselineskip] 
    \smash{\left\{ \IEEEstrut[12\jot] \right.} \nonumber \\*
    [-0.525\normalbaselineskip]
    & b & = & g(x,u) \\* 
    & y_{\theta} & = & h(x) 
\end{IEEEeqnarray}
```

## 文件头代码汇总
```latex
% add packages
\usepackage{amsmath}
\usepackage{IEEEtrantools}

% add hyperlink for equation citation
\usepackage[colorlinks=true,linkcolor=blue]{hyperref}

% page-break permission : 0 :yes  10000:no  other:yes but let latex find someother way
\interdisplaylinepenalty=2500

% fix hyperlink to invisible equation number
\makeatletter
\def\IEEElabelanchoreqn#1{\bgroup
\def\@currentlabel{\p@equation\theequation}\relax
\def\@currentHref{\@IEEEtheHrefequation}\label{#1}\relax
\Hy@raisedlink{\hyper@anchorstart{\@currentHref}}\relax
\Hy@raisedlink{\hyper@anchorend}\egroup}
\makeatother
\newcommand{\subnumberinglabel}[1]{\IEEEyesnumber
  \IEEEyessubnumber*\IEEElabelanchoreqn{#1}}

% fix font behavior of IEEEeqnarray
\renewcommand{\theequationdis}{{\normalfont (\theequation)}} 
\renewcommand{\theIEEEsubequationdis}{{\normalfont (\theIEEEsubequation)}} 

% fix distance between f and ()
\usepackage{mleftright}
\mleftright

% fix ()paris in different line same size
\newcommand{\sizecorr}[1]{\makebox[0cm]{\phantom{$\displaystyle #1$}}}

% argmin argmax  trace
\DeclareMathOperator*{\argminA}{arg\,min}
\DeclareMathOperator*{\argminB}{argmin}
\DeclareMathOperator*{\argmaxA}{arg\,max}
\DeclareMathOperator*{\argmaxB}{argmax}

\DeclareMathOperator{\tr}{Tr}

% fix d in integration
\newcommand{\dd}{\mathop{}\!\mathrm{d}}

```


*欢迎留言讨论。*