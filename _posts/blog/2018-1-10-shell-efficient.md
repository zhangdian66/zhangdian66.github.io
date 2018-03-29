---
layout: post
title: shell效率
description: 提高shell执行效率
category: blog
---

##影响因素
* 每句话都是一个进程，进程创建就需要资源
* 顺序执行，无法并发

##优化点
1. 循环
在awk内进行循环会大大减少时间
2. 正则优化
3. 减少管道与重定向
减少管道传递数据的规模，尽量使用同一个命令的选项进行数据处理
4. 尽量将程序段进行并行执行，减少运行时间
5. 各种命令的优化，这个需要对每个命令有深入的了解和实践
6. 需要多实践才行


p
参考文献:
1: [三江小渡](http://blog.pureisle.net/archives/1415.html)
2: [沐星晨](http://blog.csdn.net/sosodream/article/details/6276758)

