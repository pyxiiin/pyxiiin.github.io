---
title: '快速排序：原理、C++ 实现与面试要点'
description: '用 C++ 讲清楚快速排序的分治思想、分区写法、复杂度，以及面试常问坑点'
date: '2026-11-02T22:50:26+08:00'
slug: 'quicksort'
categories: [算法]
tags: [C/C++, 面经, 快速排序]
draft: false
---

## 核心思想

快速排序是典型的**分治**算法：

1. 选一个基准值（pivot）。
2. 把数组分成两半：左边都 ≤ pivot，右边都 ≥ pivot。
3. 对左右两边递归排序。

和归并排序不同，快排的“分”比较贵（分区），“合”几乎免费（原地完成，无需合并）。

## C++ 实现（Lomuto 分区）

面试里最常见、最好讲清楚的是 Lomuto 分区：从左往右扫，把小于等于 pivot 的元素换到前面。

```cpp
#include <vector>
#include <algorithm>

int partition(std::vector<int>& a, int left, int right) {
    int pivot = a[right];          // 选最右边作为基准
    int i = left;                  // i 指向「小于等于区」的下一个空位
    for (int j = left; j < right; ++j) {
        if (a[j] <= pivot) {
            std::swap(a[i], a[j]);
            ++i;
        }
    }
    std::swap(a[i], a[right]);     // 把 pivot 放到最终位置
    return i;
}

void quickSort(std::vector<int>& a, int left, int right) {
    if (left >= right) return;
    int p = partition(a, left, right);
    quickSort(a, left, p - 1);
    quickSort(a, p + 1, right);
}

void quickSort(std::vector<int>& a) {
    if (a.empty()) return;
    quickSort(a, 0, static_cast<int>(a.size()) - 1);
}
```

跑一遍小例子，心里会更清楚：

```text
[3, 6, 8, 10, 1, 2, 1]  pivot = 1（最后一个）
分区后大致变成：
[1, 1, ...] 再分别递归左右两侧
```

## 复杂度

| 情况 | 时间 | 说明 |
| --- | --- | --- |
| 最好 / 平均 | O(n log n) | 每次大致对半分 |
| 最坏 | O(n²) | 数组已有序，且总选端点作 pivot |
| 空间 | O(log n) 平均 | 递归栈；最坏 O(n) |

原地排序，额外空间主要来自递归调用栈，不需要像归并那样再开一个等长数组。

## 面试常问

**1. 为什么最坏会退化成 O(n²)？怎么缓解？**

已排序数组 + 固定选端点作 pivot 时，每次只缩小 1，递归深度变成 n。常见缓解：

- 随机选 pivot
- 三数取中（首、中、尾取中位数）
- 数据接近有序时改用插入排序（许多标准库实现会这么做）

**2. 快排稳定吗？**

不稳定。相等元素在分区交换时可能改变相对顺序。

**3. 和 `std::sort` 的关系？**

C++ 的 `std::sort` 要求平均 O(n log n)，实现通常是**内省排序**（Introsort）：快排为主，递归太深就切到堆排序，小区间用插入排序。所以面试里说“`std::sort` 就是快排”不准确，说“以快排为核心的混合排序”更合适。

**4. 和归并、堆排怎么选？**

- 要稳定、或链表场景：更常想到归并
- 只要前 k 大 / 保证最坏 O(n log n)：堆排序思路有用
- 一般数组原地排序、平均性能优先：快排（或标准库的 `std::sort`）

## 小结

快排的关键是**分区**：把 pivot 放到最终位置，并保证左右有序区间的边界正确。代码写对之后，面试重点通常不在背模板，而在讲清最坏情况、稳定性，以及和标准库排序的关系。
