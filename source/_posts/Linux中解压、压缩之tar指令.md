---
title: Linux中解压、压缩之tar指令
date: 2023-05-03 19:30:48
categories:
- linux
tags:
- linux
---

## 前言
`tar`指令是Linux系统最常用的打包命令，
下面举例说明一些`tar`的`基本案例`。
## 打包
### `-cvf`**仅打包**，不压缩
```bash
# 将abc文件夹打包，并指定名称为temp.tar
tar -cvf temp.tar ./abc
```
### `-zcvf`**打包并压缩**，增加`z`选项，结果文件后缀一般为`.tar.gz`或`.tgz`
```bash
# 将abc文件夹打包并压缩，并指定名称为temp.tar.gz
tar -zcvf temp.tar.gz ./abc
```
## 解包
### `-xvf`仅解包，对于`tar`文件使用
```bash
# 将temp.tar文件解包到当前目录
tar -xvf temp.tar
```
### `-zxvf`解压缩，对于`.tar.gz`或`.tgz`文件使用
```bash
# 将temp.tar文件解压到当前目录
tar -zxvf temp.tar.gz
```
## 指令详解

 - `-c` (create：创建)，**创建新的包**
 - `-v` (verbose：冗长的)，**显示tar指令处理时的详细信息**
 - `-f` (file：文档)，**指定tar包文件名**
 - `-x` (extract：提取)，**进行解压缩**
 - `-z` 调用gzip程序来压缩文件，压缩后的文件名称以.gz结尾
 - `-r` 向压缩文件后追加新的文件

> `c`与`x`不可同时使用

## 总结
 - `*.tar`类型文件用 `tar -xvf`解包
 - `.tar.gz`和`.tgz` 用 `tar -xzf`解压


