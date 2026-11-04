---
title: '快速排序算法：原理与C++实现'
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

## 实现原理（挖坑法）
1. 设置两个变量 low、high，排序开始时：low=0，high=size-1。
2. 整个数组找基准正确位置，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面
    - 2.1 默认数组的第一个数为基准数据，赋值给key，即key=array[low]。
    - 2.2 因为默认数组的第一个数为基准，所以从后面开始向前搜索（`high--`），找到第一个小于key的array[high]，就将 array[high] 赋给 array[low]，即 array[low] = array[high]。（循环条件是 array[high] >= key；结束时 array[high] < key）
    - 2.3 此时从前面开始向后搜索（`low++`），找到第一个大于key的array[low]，就将 array[low] 赋给 array[high]，即 array[high] = array[low]。（循环条件是 array[low] <= key；结束时 array[low] > key）
    - 2.4 循环 2.2-2.3 步骤，直到 low=high，该位置就是基准位置。
    - 2.5 把基准数据赋给当前位置。
3. 第一趟找到的基准位置，作为下一趟的分界点。
4. 递归调用（recursive）分界点前和分界点后的子数组 排序，重复2、3、4的步骤。
5. 最终就会得到排序好的数组。

```cpp
#include <vector>
using namespace std;

int getStandard(vector<int> &nums, int left, int right) {
    int key = nums[left];
    while (left < right) {
        while (left < right && nums[right] >= key) {
            --right;
        }
        if (left < right) {
            nums[left] = nums[right];
        }
        
        while (left < right && nums[left] <= key) {
            ++left;
        }
        if (left < right) {
            nums[right] = nums[left];
        }
        
    }
    nums[left] = key;
    return left;
}

void quickSort(vector<int> &nums, int left, int right) {
    if (left < right) {
        int standard = getStandard(nums);
        quickSort(nums, left, standard-1);
        quickSort(nums, standard+1, right);
    }
}
```


## 实现原理（Lomuto 分区）
1. 设置两个变量 left、right，排序开始时：left=0，right=size-1。
2. 整个数组找基准正确位置，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面
    - 2.1 默认数组的最后一个数为基准数据，赋值给pivot，即pivot=array[right]。
    - 2.2 设置变量 i=left，i 表示「小于等于区」的下一个空位。
    - 2.3 用 j 从 left 扫描到 right-1（只从左往右扫一遍）。
    - 2.4 如果 array[j] <= pivot，就把 array[i] 和 array[j] 交换，然后 i++；如果 array[j] > pivot，什么都不做，继续 j++。
    - 2.5 扫描结束后，把 array[i] 和 array[right] 交换，把基准放到最终位置；此时 i 就是基准位置，左边都 ≤ pivot，右边都 > pivot。
3. 第一趟找到的基准位置，作为下一趟的分界点。
4. 递归调用（recursive）分界点前和分界点后的子数组排序，重复2、3、4的步骤。
5. 最终就会得到排序好的数组。

## C++ 实现（Lomuto 分区）

下面代码对应上一节的 Lomuto 分区：从左往右扫，把小于等于 pivot 的元素换到前面。

```cpp
#include <vector>
#include <algorithm>

int getStandard(vector<int> &nums, int left, int right) {
    int key = nums[right];
    int i = left;
    for (int j = left; j < right; ++j) {
        if (nums[j] <= key) {
            swap(nums[i], nums[j]);
            ++i;
        }
    }
    swap(nums[i], nums[right]);
    return i;
}

void quickSort(vector<int> &nums, int left, int right) {
    if (left < right) {
        int standard = getStandard(nums, int left, int right);
        quickSort(nums, left, standard-1);
        quickSort(nums, standard+1, right);
    }
}
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

- **核心**：选基准、分区、递归。分区一趟结束后，基准就落在最终位置，左边都不大于它，右边都不小于它。
- **两种分区**：挖坑法基准取第一个，从右边先搜；Lomuto 基准取最后一个，从左往右扫一遍。它们只是分区手法不同，整体的分治思路一样。
- **复杂度**：平均 O(n log n)，最坏 O(n²)，原地排序，额外空间主要是递归栈。
- **面试重点**：最坏情况怎么出现、怎么用随机基准或三数取中缓解、快排不稳定，以及 `std::sort` 是以快排为核心的混合排序。
