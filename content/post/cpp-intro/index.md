---
title: 'C/C++ 学习笔记：开篇'
description: '为什么还要学 C/C++，以及这个系列打算写什么'
date: '2026-11-02T22:09:29+08:00'
slug: 'cpp-intro'
categories: [C/C++]
tags: [C, C++, 学习路线]
draft: false
---

## 为什么还要学 C/C++

在高级语言层出不穷的今天，C/C++ 依然是很多领域绕不开的基础：

- **操作系统与嵌入式**：Linux 内核、驱动、单片机程序，几乎都是 C 写的。
- **高性能场景**：游戏引擎、数据库、浏览器内核、推理框架，对性能要求极高的地方大多是 C++。
- **理解计算机**：指针、内存布局、编译链接这些概念，学 C/C++ 时躲不开，学懂了再看其他语言会清楚很多。

## 这个系列打算写什么

1. **基础回顾**：指针与引用、内存管理、编译与链接过程。
2. **现代 C++**：智能指针、移动语义、Lambda、STL 常用容器与算法。
3. **工程实践**：CMake 构建、调试技巧（GDB）、常见内存问题排查。
4. **踩坑记录**：实际写代码时遇到的问题和解决方法。

## 先来一段代码

用现代 C++ 写的 Hello World，顺便用上智能指针：

```cpp
#include <iostream>
#include <memory>
#include <string>

int main() {
    auto greeting = std::make_unique<std::string>("Hello, C++!");
    std::cout << *greeting << std::endl;
    return 0;  // greeting 离开作用域时自动释放，无需手动 delete
}
```

编译运行：

```bash
g++ -std=c++17 -Wall hello.cpp -o hello && ./hello
```

后面的文章会一点点展开，欢迎一起交流。
