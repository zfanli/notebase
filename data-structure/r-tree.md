---
type: DataStructure
tags: DataStructure Algorithm
created: 2022-02-23
---

# R-tree

R-tree 的 R 表示 Rectangle，即矩形。

R-tree 是一种树状结构，用来实现访问空间信息的方法。比如，对地理坐标、四边形或多边形等多维信息进行索引。

R-tree 是高级高度平衡树结构（**Advanced height-balanced Tree Data Structure**）。

R-tree 是多维扩展版本的 [[b-tree|B-tree]]。其也是一种平衡搜索树，所有叶子节点都在同一深度，将数据分页组织，被设计用来存储数据到磁盘（在数据库中使用）。

一个典型的例子，比如使用 R-tree 查找某个位置附近 20 里内的所有饭店。

- [[minimum-bounding-rectangle|MBR]]

## R-tree 的思路

R-tree 这种数据结构的关键思路在于将临近的对象分组，在更高层级上将它们表达为 [[minimum-bounding-rectangle|MBR]]，即最小外接矩形。这也是 R-tree 名字中 R （Rectangle）的来源。

而正因为所有对象都在外接矩形之内，如果一个查询不与外界矩形相交，那么这个查询不可能与外接矩形中的任何对象相交。

R-tree 的叶子节点的每个矩形描述一个单独的对象，从叶子节点往上逐步聚集更多的对象。这可以视为数据集的持续粗近似（increasingly coarse approximation）。
