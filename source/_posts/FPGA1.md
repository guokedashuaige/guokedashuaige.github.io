---
title: FPGA杂谈1
date: 2019-12-26 10:47:07
tags:
  - Chips
  - FPGA
  - Fintech
categories: FPGA
mathjax: true
---
更新中
# 计算机架构的黄金时代

 
## ISA
软件与硬件的通信是通过指令集架构（ISA，Instruction Set Architecture）进行的.在 1960 年代早期，IBM 有四个互不兼容的计算机产品线，每个都有自己的 ISA、软件堆栈、I/O 系统和利基市场（分别针对的是小型企业、大型企业、科研和实时应用）。大家认为需要一种方案，让便宜的 8 位数据路径计算机与高速的 64 位数据路径计算机都能共用一个ISA。 Maurice Wilkes 提出了一种简化控制硬件的方法。控制可被描述为一个二维数组，他称之为控制存储器（control store）」。这个数组的每一列都对应于一条控制线，每一行都是一个微指令（microinstruction），而编写微指令则被称为微编程（microprogramming）。一个控制存储器包含一个用微指令编写的 ISA 解释器，所以执行一个常规指令需要多个微指令。这种控制存储器是通过内存实现的，成本比逻辑门要低得多。![](http://deliveryimages.acm.org/10.1145/3290000/3282307/uf2.jpg)
 Table 1. Features of four models of the IBM System/360 family; IPS is instructions per second.
表格1列出了 IBM 在1964年4月7日宣布的System/360 ISA的4种型号。数据路径的变化范围有 8 倍，内存容量的变化范围有16倍，时钟频率是4倍，性能是50倍，成本接近6倍。成本最高的计算机的控制存储器最宽，因为更复杂的数据路径使用更多控制线。成本最低的计算机因为硬件更简单而有更窄的控制存储器，但因为它们需要更多时钟周期来执行一个 System/360 指令，所以需要更多微指令。
## 从IC到IBM PC
当计算机开始使用集成电路时，摩尔定律意味着控制存储器可以变大很多。更大的内存反过来又意味着允许使用更复杂的ISA。
某些制造商选择开放微编程功能，让选定的客户能添加定制功能，他们称之为「可写控制存储器（WCS）。最有名的 WCS 计算机是 Alto，是第一款个人计算机（PC），配备有首款位映像显示器（bit-mapped display）和首个以太网局域网。用于这种全新显示器和网络的设备控制器是存储在一个 4096 字×32 位 WCS 中的微程序。
之后便是英特尔抓住机会用8086获得了给IBM供货的机会。1970 年代的微处理器（比如英特尔的 8080）仍处于 8 位时代，主要依靠汇编语言编写程序。到了1980年代，英特尔把8086的寄存器和指令集扩展成了 16 位。IBM 当时正在开发一款个人计算机来与 Apple II 竞争，最终采用了8086的8位总线版本，在全球售出了1亿台。
## CISC Vs RISC

## 从复杂指令集到精简指令集计算机


# References：
[1. Hennessy, J. L., & Patterson, D. A. J. C. A. (2019). A new golden age for computer architecture. 62(2), 48-60.](https://cacm.acm.org/magazines/2019/2/234352-a-new-golden-age-for-computer-architecture/fulltext) 
