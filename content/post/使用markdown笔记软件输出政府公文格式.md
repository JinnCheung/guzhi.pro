---
title: "用Markdown笔记软件输出政府公文格式"
date: 2021-03-31T23:41:24+08:00
tags:
  - "公务程序员"
categories:
  - "办公自动化"
---
> 这是[估值PRO](guzhi.pro)的第4篇文章，也是[公务程序员](/tags/公务程序员/)系列（一）。
>
> 更新：2021-03-31

习惯了用Markdown写文章，渐渐不大喜欢用Word、wps这些重型编辑器，但平时工作又避不开写各种公文，因此琢磨了一下，在Typora 和 IA writer两款比较流行的Markdown编辑器上输出政府公文的方法。

<!--more-->

## 一、基本概念

- 印刷单位：pt & 字号

- 屏幕显示单位：px

- 屏幕显示密度：DPI





一、windows mac 和打印字体

pt (point，点、磅) 是一个物理长度单位，指的是72分之一英寸。

字号是中文字库中特有的一种单位，以中文代号表示特定的磅值pt，便于记忆、表述。

公文字体主要两个号

pt = 1/72 inch  px = 1/DPI  inch

72pt = 1 Inch 96px= 1 Inch

1pt = 96 * px / 72

## 四号 = 14pt =18px（96dpi）= 14px（72dpi）

## 三号 = 16pt = 21 px（96dpi）= 16px（72dpi）

## 行高 = 28pt = 37px（96dpi） = 28px(72Dpi)

## 二号 = 22pt = 29px（96dpi）  = 22px(72Dpi)

## 21CM = 596pt  = 794px（96dpi）= 596px(72Dpi)

## 29.7CM =  842pt = 1123px（96dpi）= 842px(72Dpi)

## 15.6CM = 590px（96dpi）=442px(72Dpi) = 442pt 

## 22.5CM = 850px（96dpi）=638px(72Dpi)=638pt 

410

v590/21

## 3.7 CM = 105pt

```css
html {
    font-size: 16pt; /*三号字（16pt）*/
    --line-height-base: 175;  /*行高28pt（16*175%）*/
}
body {
    max-width: 27.625em;
    padding-top: 105pt;
}
@media print {
    body {
        max-width: 410pt;
        padding-top: 0;
    }
  }
  
body.footer{
    padding-top: 0;
}

h1 {
    font-size: 22pt; /*二号字（22pt）*/
    font-family: "方正小标宋简体";
    font-weight: normal;
    text-align: center;
    margin-bottom: 22pt;
}

h2, h3, h4, p{
    margin-top: 0rem;
    margin-bottom: 0rem;
    text-indent: 2rem;
    text-align: justify;
    line-height: 28pt;
}
h2{
    font-family: "黑体";
}
h3{
    font-family: "楷体_GB2312","楷体";
}
h4{
    font-weight: bold;
    font-style: normal;
}
h4, p {
    font-family: "仿宋_GB2312","仿宋";
}
.footer p{
    text-align: center;
} 
.footer span{
    font-size: 14pt;
}
```