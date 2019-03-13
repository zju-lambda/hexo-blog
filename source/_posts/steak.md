---
title: Steak 语言的设计与实现
tags: 
    - c++
    - functional programming
mathjax: true
category: nicekingwei
date: 2019-03-13 23:00:03
redirect: https://zhuanlan.zhihu.com/p/59182783
---

阅读本文需要读者了解一些基本的函数式语言概念和 C++ 11 知识。
Steak 语言是我两年前写的一个项目，最近又翻出来优化了一下，早就说要写篇文章总结一下，结果一直咕咕咕。
Steak 语言设计的初衷是把 GADT、惰性求值、模式匹配等函数式语言特性引入 C++。C++ 11 以后，语言里逐渐出现了一些支持函数式编程的特性，例如 lambda 表达式，但他们用起来又不是特别好用（等以后有了 Ranges，情况会好转一些）。因此我想设计一个库或语言，让 C++ 使用者能更舒服地编写函数式程序。当然目前的结果只是个玩具，因为编译太慢了：）。