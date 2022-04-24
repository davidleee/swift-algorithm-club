![Swift Algorithm Club](Images/SwiftAlgorithm-410-transp.png)

# 欢迎来到 Swift 算法俱乐部

在这里，你能找到各种知名算法和数据结构的 Swift 版本实现（对，就是那个深受大家喜爱的编程语言），并附带了详细的说明来解释它们是如何运作的。

如果你是一名需要这类知识来应付考试的计算机科学系学生，或者你是一名想要夯实理论基础的自学型程序员，那你就算来对地方了！

这个项目的目标是 **解释算法的实现原理**。关注点在于让代码更加清晰易读，而不是实现一个可以直接用在你自己项目里的依赖库。也就是说，虽然这里的大多数代码都是可以在实际场景中使用的，但是在你粘贴过去之后千万要注意检查它们是不是真的合适。

这里的代码基于 **Xcode 10** 和 **Swift 4.2** 编写。

**欢迎留下你的建议或参与共建！**

## 重要的链接

[什么是算法和数据结构？](What%20are%20Algorithms.markdown) 看菜谱做菜！

[为什么要学习算法？](Why%20Algorithms.markdown) 如果你在犹豫这些内容是否适合自己，来看看这个。

[算法复杂度的标记方法](Big-O%20Notation.markdown)。我们经常说“这个算法的复杂度是O(n)”。如果你不太清楚这是什么意思，看看这个。

[算法设计的技巧](Algorithm%20Design.markdown)。如何创造一个新的算法？

[欢迎参与共建](https://github.com/raywenderlich/swift-algorithm-club/blob/master/.github/CONTRIBUTING.md)。你可以通过创建 issue 或 Pull Request 的方式给这个项目提供帮助。

## 从哪里开始？

如果你刚刚入门算法和数据结构，可以先从下面几个知识点开始了解：

- [栈](Stack/)
- [队列](Queue/)
- [插入排序](Insertion%20Sort/)
- [二分查找](Binary%20Search/) 和 [二分查找树](Binary%20Search%20Tree/)
- [归并排序](Merge%20Sort/)
- [摩尔字符串搜索](Boyer-Moore-Horspool/)

## 算法

### 查找

- [线性查找](Linear%20Search/)。在一个数组里查找某个元素。
- [二分查找](Binary%20Search/)。在一个有序数组中快速查找元素。
- [计算出现次数](Count%20Occurrences/)。计算某个值在数组中出现的次数。
- [查找最小值/最大值](Select%20Minimum%20Maximum)。计算数组中的最小值/最大值。
- [第 k 大的元素](Kth%20Largest%20Element/)。查找数组中第k大的元素，比如中位数。
- [选择抽样](Selection%20Sampling/)。在集合中随机选取一批元素。
- [查找集合](Union-Find/)。跟踪集合间不重复的元素，以便能快速合并多个集合。


### 字符串搜索

- [穷举法](Brute-Force%20String%20Search/)。一种简单直白的方法。
- [摩尔算法](Boyer-Moore-Horspool/)。一种快速搜索子串的方法。它借助一张查询表来跳过某些字符，从而避免对每个字符都进行处理。
- [KMP 算法](Knuth-Morris-Pratt/)。一种有着线性时间复杂度的算法，可以返回给定规律的子串的出现下标。
- [Rabin-Karp 算法](Rabin-Karp/)。一种基于哈希的更快的查询方法。
- [最长相同子串](Longest%20Common%20Subsequence/)。在不同字符串中，找寻最长的相同部分。
- [Z 算法](Z-Algorithm/)。在字符串中找到所有具有特定排列模式的子串，并返回这些子串的起始位置下标。

### 排序

了解排序算法的实现是很有意思的，但在实际使用过程中你很少需要自己写排序流程。Swift 提供的 `sort()` 方法已经完全可以胜任这个工作了。但是如果你对实现感到好奇，那么请继续往下看...

基础排序：

- [插入排序](Insertion%20Sort/)
- [选择排序](Selection%20Sort/)
- [希尔排序](Shell%20Sort/)

快速排序：

- [快速排序](Quicksort/)
- [归并排序](Merge%20Sort/)
- [堆排序](Heap%20Sort/)

动态排序：

- [内省排序](Introsort/)

出于特殊目的的排序：

- [计数排序](Counting%20Sort/)
- [基数排序](Radix%20Sort/)
- [拓扑排序](Topological%20Sort/)

不太好的排序算法（别用！）：

- [冒泡排序](Bubble%20Sort/)
- [慢速排序](Slow%20Sort/)

### 压缩

- [游程编码（RLE）](Run-Length%20Encoding/)。将重复的数值保存为一个字节和一个个数标记。
- [哈夫曼编码](Huffman%20Coding/)。用更少的比特位来保存更多常见元素。

### 混淆

- [“洗牌”（Shuffle）](Shuffle/)。随机打乱一个数组。
- [梳排序](Comb%20Sort/)。冒泡排序的优化版。
- [凸包](Convex%20Hull/)。
- [米勒-拉宾素性检验](Miller-Rabin%20Primality%20Test/)。这个数字是素数吗？
- [最少硬币兑换](MinimumCoinChange/)。动态编程的一个示例。
- [遗传算法](Genetic/)。参考生物进化理论实现的一个数值最佳化的简单例子。 A simple example on how to slowly mutate a value to its ideal form, in the context of biological evolution.
- [Myers 差分算法](Myers%20Difference%20Algorithm/)。找寻两个序列中的最长相同子序列。

### 数学理论

- [最大公约数（GCD）](GCD/)。附加奖励：最小公倍数。
- [排列组合](Combinatorics/)。组合数学启动！
- [调度场算法](Shunting%20Yard/)。将中缀表达式转换为后缀表达式的经典算法。
- [卡拉楚巴算法](Karatsuba%20Multiplication/)。另一种快速乘法算法。
- [半正矢公式](HaversineDistance/)。计算圆上两点距离的方式。
- [施特拉森矩阵乘法](Strassen%20Matrix%20Multiplication/)。一种高效的矩阵乘法算法。
- [逆时针算法](/CounterClockWise/)。计算简单多边形的面积。

### 机器学习

- [k 平均算法](K-Means/)。把点划分到 k 个聚类中的算法。
- k 邻近算法
- [线性回归](Linear%20Regression/)。对一个（或多个）变量之间关系进行建模的一种回归分析。
- 逻辑回归
- 神经网络
- 网页排名算法
- [朴素贝叶斯分类器](Naive%20Bayes%20Classifier/)
- [模拟退火](Simulated%20annealing/)。一种在很大搜寻空间中寻找近似最优解的通用概率算法。

## 数据结构

为特定任务挑选合适的数据结构取决于下面几件事情。

第一，数据组成形式选择与你使用它们的方式相关。如果你想通过关键字去查找对象，那么你需要一个字典（dictionary）；如果你的数据可以被组成上下级，那么你大概会用到某种树型结构；如果你的数据是有序的，那么也许栈或队列更适合你。

第二，考虑最频繁的使用方式是什么，因为某些数据结构是针对特定动作进行过优化的。比方说，如果你经常要在某个集合里找到最重要的某个元素，那么堆或者优先队列会比一个普通的数组性能更好。

大多数时候，使用内置的 `Array`、`Dictonary` 和 `Set` 已经足够了，但有些时候你可能想要用一些更酷炫的方式...

### 数组的各种变形

- [二维数组](Array2D/)。一种维度固定的二维数组。棋牌类游戏中常常会用到。
- [比特位集合](Bit%20Set/)。 一种保存了 *n* 个比特位的定长序列。
- [定长数组](Fixed%20Size%20Array/)。当你提前知道要存储的数据长度时，用复古的固定长度的数组也许会更加高效。
- [有序数组](Ordered%20Array/)。一种一直保持某种顺序的数组。
- [Rootish Array Stack](Rootish%20Array%20Stack/)。一种时间和空间上都更高效的 Swift 数组变体。

### 队列

- [栈](Stack/)。后进，先出！
- [队列](Queue/)。先进，先出！
- [双端队列](Deque/)。一种有两个出入口的队列。
- [优先队列](Priority%20Queue)。一种确保最重要的元素永远在最前面的队列。
- [环形缓冲区](Ring%20Buffer/)。也叫循环缓冲区。一种有固定长度且首尾相接的数组。

### 链表

- [链表](Linked%20List/)。一组通过链接组合在一起的数据元素。包括单向和双向链表。
- [跳跃列表](Skip-List/)。一种基于概率的数据结构，与 AVL 数和红黑树有相同的算法时间复杂度，并且能很好地平衡搜索和数据更新上的效率。

### 树

- [树](Tree/)。一种常规概念上的树型结构。
- [二叉树](Binary%20Tree/)。一种每个节点最多只能有两个子节点的树。
- [二叉搜索树（BST）](Binary%20Search%20Tree/)。一种经过特殊排序的二叉树，能用于更快速的查询。
- [红黑树](Red-Black%20Tree/)。一种自平衡的二叉搜索树。
- [伸展树](Splay%20Tree/)。一种自平衡的二叉搜索树，能快速找回最近更新的元素。
- [线索二叉树](Threaded%20Binary%20Tree/)。一种维护了少量额外变量的二叉树，能在节点间进行快速的有序移动。
- [线段树](Segment%20Tree/)。可以快速查询结构内包含某一点的所有区间。
  - [惰性传播](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Segment%20Tree/LazyPropagation)
- kd 树
- [稀疏表](Sparse%20Table/)。与线段树的功效相同，但这次会更快！
- [堆](Heap/)。一种以数组形式保存的二叉树，所以它不需要用到指针。是一种很好的优先队列。
- 斐波那契堆
- [前缀树](Trie/)。一种用于保存关联数据结构的特殊的树。
- [B 树](B-Tree/)。一种自平衡搜索树，每个节点可以拥有2个以上的子节点。
- [四叉树](QuadTree/)。每个节点正好有4个子节点的树。
- [八叉树](Octree/)。每个节点正好有8个子节点的树。

### 哈希

- [哈希表](Hash%20Table/)。让你能够用关键字来存取对象。字典通常就是基于哈希表实现的。
- 散列函数

### 集合

- [布隆过滤器](Bloom%20Filter/)。一种占用固定内存的数据结构，用于检索一个元素是否在一个集合中。
- [哈希集合](Hash%20Set/)。一种基于哈希表实现的集合。
- [多重集](Multiset/)。一种允许同一元素出现多次的集合，也叫做“包”。
- [有序集合](Ordered%20Set/)。一种有序的集合。

### 图

- [图](Graph/)
- [广度优先搜索（BFS）](Breadth-First%20Search/)
- [深度优先搜索（DFS）](Depth-First%20Search/)
- [最短路径](Shortest%20Path%20%28Unweighted%29/)，在非平衡树上实现。
- [单源最短路径](Single-Source%20Shortest%20Paths%20(Weighted)/)
- [最小生成树](Minimum%20Spanning%20Tree%20%28Unweighted%29/)，在非平衡树上实现。
- [最小生成树](Minimum%20Spanning%20Tree/)
- [任意两点之间的最短路径](All-Pairs%20Shortest%20Paths/)
- [迪杰斯特拉算法](Dijkstra%20Algorithm/)
- [A*算法](A-Star/)

## 算法题

许多软件开发岗位的面试都有做算法题的环节。这里列举了一小部分有趣的题目。想找更多算法题（或答案）可以来[这里](http://elementsofprogramminginterviews.com/)和[这里](http://www.crackingthecodinginterview.com)。

- [Two-Sum Problem](Two-Sum%20Problem/)
- [Three-Sum/Four-Sum Problem](3Sum%20and%204Sum/)
- [Fizz Buzz](Fizz%20Buzz/)
- [Monty Hall Problem](Monty%20Hall%20Problem/)
- [Finding Palindromes](Palindromes/)
- [Dining Philosophers](DiningPhilosophers/)
- [Egg Drop Problem](Egg%20Drop%20Problem/)
- [Encoding and Decoding Binary Tree](Encode%20and%20Decode%20Tree/)
- [Closest Pair](Closest%20Pair/)

## 继续学习！

对上面的内容还满意吗？那就来看看 [Data Structures & Algorithms in Swift](https://store.raywenderlich.com/products/data-structures-and-algorithms-in-swift)，这是 Swift 算法俱乐部团队官方出版的书籍。

![Data Structures & Algorithms in Swift Book](Images/DataStructuresAndAlgorithmsInSwiftBook.png)

你会从基础的数据结构开始，例如链表、队列、栈，学习如何用 Swift 的语法形式来实现它们。接着会学习各种类型的树结构，包括一些具有特定目标的树，比如二叉树、AVL 树、二叉搜索树和前缀树。

你还能看到一些比冒泡排序和插入排序性能更好的算法，比如归并排序、基数排序、堆排序和快速排序。你还可以学习到：如何构造有向、无向和加权图，用以表示现实世界中的模型；使用广度优先、深度优先、迪杰斯特拉和普林姆算法来高效地翻转图或树，用以解决类似最短路径或最低成本网络的问题。

读完这本书，你就拥有了使用数据结构和算法解决常见问题的实操经验，而且你还拥有了创造属于自己的高效算法的能力。

你可以在 [raywenderlich.com 商城](https://store.raywenderlich.com/products/data-structures-and-algorithms-in-swift)里找到这本书。

## 鸣谢

Swift 算法俱乐部最初由 [Matthijs Hollemans](https://github.com/hollance) 创建。

现在由 [Vincent Ngo](https://www.raywenderlich.com/u/jomoka)、 [Kelvin Lau](https://github.com/kelvinlauKL) 和 [Richard Ash](https://github.com/richard-ash) 共同维护。

Swift 算法俱乐部是 [raywenderlich.com](https://www.raywenderlich.com) 社区中的[众多算法同好们](https://github.com/raywenderlich/swift-algorithm-club/graphs/contributors)一起努力的成果。期待你的参与 —— [加入俱乐部](.github/CONTRIBUTING.md)？:]

本翻译页面由 [David Lee](https://github.com/davidleee) 创建与维护。

## 许可

All content is licensed under the terms of the MIT open source license.

By posting here, or by submitting any pull request through this forum, you agree that all content you submit or create, both code and text, is subject to this license.  Razeware, LLC, and others will have all the rights described in the license regarding this content.  The precise terms of this license may be found [here](https://github.com/davidleee/swift-algorithm-club/blob/cn-translation/LICENSE.txt).
