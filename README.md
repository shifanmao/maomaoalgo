阿🐱阿🐶=maomao algo=毛毛的算法笔记

##### Table of Contents  
[关于本书](#关于本书)  
[DFS](#dfs)  

# 关于本书
作为一个算法小白，在力扣和书本之间来回，看了很多大神耐心的解答和总结，书本里循循善诱的知识整理。总想用一张表格来梳理出算法的知识体系，却也一直没有时间整理。这本笔记里，我选择用比较简单的Python语言来回顾一下数据结构和算法的基本知识。

目的不是覆盖所有的知识点，而是想要把自己读的书越读越薄。用尽量浅显的理解来实现比较常见和实用的数据结构和算法，也当是对自己学习的总结，可能能帮到读者。当中有什么疏漏和错误可以直接评论。

## 为什么选择Python？
当然是因为好写啦！

平时在阅读的时候发现，很多经典算法书都是用Java或C++演示代码的，虽然也方便理解，但是用Python来作为范例的不算多数。所以我们主要来做两件事：

1. 用Python简单实现基础的数据结构。有些数据结构（比如说OrderedDict）都现成调库就行了，但是为了理解它们的读写速度啊，优缺点啊，用相对简单的数据结构（数组和链表）来简单实现能帮助理解。
2. 介绍基本算法，提供一些算法模版。（1）搜索方法，（2）用动态规划来优化搜索速度，（3）如何求解图的性质或最值，（4）如何用数据流或多个机器来再加速。算法主要是提供一些思维方式，让我们从“能解决“到“能高效解决“地问题。

## 算法总结
很多算法都有“一脉相承“的思路。所以这里我把算法的介绍顺序变成：搜索算法-->动态规划-->图论，因为我觉得有些算法这样引出这样比较好理解。

| 引子      | 算法 |
| ----------- | ----------- |
| [DFS](#dfs)      | (DP)[#dp] / 可以理解为带备忘录的DFS（Memoization）  |

# DFS
```python
def dfs(x):
  if x ...: # base case condition
    return ... # base case

  res = ... # initialization
  ... # business lgic
  return res
```

### DFS Example 1:
在一家公司中，我们需要知道每个员工有几个下属。那么我们给每一个员工一个分数Score：如果TA没有下级，那么Score=1，否则Score=下级的个数+1（自身）。我们把每一个员工看成一个节点，那么这个公司从CEO开始作为根节点就可以看作是一棵树。
那么问题来了：给定一个公司结构和员工号，我们如何查找TA的score？这样的问题一般用DFS来递归解决。

#### Solution：一次求解
```python
def dfs(graph, id):
    if id not in graph: return 0 # base case
    score = 1                    # 自己
    for e in graph[id]:          # 下属
        score += dfs(graph, e)
    return score
```

DFS搜索经常出现的问题就是重复查询。比如说在这个问题中，我们两次分别查询员工与TA的老板，那么员工的score就是可以重复利用的信息。对于这样的重复子问题，有两种解决方案：
* 备忘录：在自顶向下递归的最外面加一个cache（或者memo）来存储已经求解了的答案。在动态规划的介绍中我们再重温这个话题。
* 动态规划：利用子问题的结构，用自底向上的方法来消灭重复查询。比如练习《验证平衡二叉树》，不像动态规划，然而用了这样的思路。

#### Solution 2：带Cache的求解
```python
cache = {} # id: score
def dfs(graph, id):
    if id not in graph: return 0 # base case
    
    if id in cache: return cache[id]
    score = 1                    # 自己
    for e in graph[id]:          # 下属
        score += dfs(graph, e)

    cache[id] = score
    return res
```

讨论：如果面对多次查找的同时，对树的结构还可能有改变（公司变动）。那么我们需要灵活改变cache(cache invalidation)来应对。比如我们可以拓展cache来存储以下内容cache: id -> score, manager_id, set(report_id)
这样一来，我们相当于拥有了对父节点（老板）的指针。一旦有一个subtree（部门）有改变，我们就可以向上遍历，来改动他们在cache中的score。


### DFS Example 2
给定一张迷宫，入口entrance与出口exit，如何从entrance到达exit？有两个方便的函数来我们进行选择和判断：
neighbors(p)来访问从p出发相邻的点
wall(p,q)来判断每个两点p与q之间是否隔着一堵墙

#### Solution 1: 回溯
从起点出发进行搜索，而且我们的在每一个点的策略是，每次遇到选择就选择第一个可行解（neighbors(p)当中给我们的第一个结果，可以是向左转前进一格）。直到我们没有选择了再回头。如果有解（exit可到达），那么至少有一条路径是连接出入口的，我们把它记下来。

注意这里“第一个可行解“可能会排除以下选择：
* 已经走过的路：一般用path（走过的路径）或者vis（额外的标记数组）来标记
* 走不通的路：越界、隔着墙等不符合最后答案的选择。
这道题里neighbors(p)帮我们做选择，wall(p,q)帮我们排除不合法选择。

```python
def dfs(path, p):
    if p==exit:
        res.append(path)
        return
    for q in neighbors(s):
        if q in path: continue  # visited
        if wall(p, q): continue # it's a wall
        path.append(q)          # 探索
        dfs(path, q)
        path.pop()              # 回溯

res = []
dfs([enter], enter)
return res
```

#### Solution 2: 非回溯
在Python中有一个偷懒的解法。在非回溯解法中，我们+(__iadd__operator)来创造一个新的list。新的list（也需要新的空间）会来替我们搜索，就不需要回溯了。

```python
def dfs(path, p):
    if p==exit:
        res.append(path)
        return
    for q in neighbors(s):
        if q in path: continue  # visited
        if wall(p, q): continue # it's a wall
        dfs(path+[q], q)        # 新的path object!

res = []
dfs([enter], enter)
return res
```

#### Solution 3: 可行解与所有解
有的时候我们不需要找到所有可行解，而是只需找到任意可行解。那么我们早早的就可以退出递归函数。

```python
def dfs(path, p):
    if p==exit:
        res[:] = path
        return True
    for q in neighbors(s):
        if q in path: continue  # visited
        if wall(p, q): continue # it's a wall
        path.append(q)          # 探索
        if dfs(path, q):
            return True         # 找到可行解！
        path.pop()              # 回溯
    return False                # 没有可行解

res = []
dfs([enter], enter)
return res
```
