阿🐱阿🐶=maomao algo=毛毛的算法笔记

# 关于本书
作为一个算法小白，在力扣和书本之间来回，看了很多大神耐心的解答和总结，书本里循循善诱的知识整理。总想用一张表格来梳理出算法的知识体系，却也一直没有时间整理。这本笔记里，我选择用比较简单的Python语言来回顾一下数据结构和算法的基本知识。

目的不是覆盖所有的知识点，而是想要把自己读的书越读越薄。用尽量浅显的理解来实现比较常见和实用的数据结构和算法，也当是对自己学习的总结，可能能帮到读者。当中有什么疏漏和错误可以直接评论。

# 语言的选择
## 为什么选择Python？
当然是因为好写啦！

平时在阅读的时候发现，很多经典算法书都是用Java或C++演示代码的，虽然也方便理解，但是用Python来作为范例的不算多数。所以我们主要来做两件事：

1. 用Python简单实现基础的数据结构。有些数据结构（比如说OrderedDict）都现成调库就行了，但是为了理解它们的读写速度啊，优缺点啊，用相对简单的数据结构（数组和链表）来简单实现能帮助理解。
2. 介绍基本算法，提供一些算法模版。（1）搜索方法，（2）用动态规划来优化搜索速度，（3）如何求解图的性质或最值，（4）如何用数据流或多个机器来再加速。算法主要是提供一些思维方式，让我们从“能解决“到“能高效解决“地问题。

# 算法总结
很多算法都有“一脉相承“的思路。所以这里我把算法的介绍顺序变成：搜索算法-->动态规划-->图论，因为我觉得有些算法这样引出这样比较好理解。

| 引子      | 算法 |
| ----------- | ----------- |
| [DFS搜索](#dfs)      | (动态规划算法)[#dp] / DP可以理解成Memoization的迭代写法 (aka带备忘录的DFS)       |

# DFS
```python
def dfs(x):
  if x ...: # base case condition
    return ... # base case

  res = ... # initialization
  ... # business lgic
  return res
```