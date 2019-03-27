---
layout:     post
title:      火焰图
subtitle:   async-profiler+FlameGraph生成Java进程火焰图
date:       2019-03-24
author:     BY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - async-profiler
    - FlameGraph
    - Java
---

>async-profiler+FlameGraph生成Java进程火焰图


# 前言
服务器一Java进程占用cpu达到100%（未做压测、正常使用时）

通过async-profiler+FlameGraph我们可以生成该Java进程堆栈的火焰图，根据火焰图可以发现问题代码


# 快速开始
## 安装async-profiler+FlameGraph
下载[async-profiler](https://github.com/jvm-profiling-tools/async-profiler/releases)

下载[FlameGraph](https://github.com/brendangregg/FlameGraph/releases)

将下载的文件解压到服务器目录，如/opt

给async-profiler和FlameGraph目录下的sh、pl文件赋予可执行权限

在/opt/async-profiler目录下将进程号为5144的程序，采集20秒cpu数据，采集出来输出到/vdb/quote/collapsed2.txt 文件中：
```
./profiler.sh -d 20 -o collapsed -f /opt/async-profiler-1.5-linux-x64/5144.txt 5144
```
根据5144.txt生成svn火焰图：
```
./flamegraph.pl --colors=java /opt/async-profiler-1.5-linux-x64/5144.txt > /opt/FlameGraph-1.0/5144.svg
```

将火焰图下载到本地，使用浏览器打开svn文件，可以看到占用cpu最多的方法（X轴最宽，com/zhiguan/compSnsPlat/kafka/BatchQueue$DataListener.run ）
![火焰图](https://kingrush.github.io/img/async-profiler-FlameGraph-java.svg)