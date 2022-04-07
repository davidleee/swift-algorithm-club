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

- [堆栈](Stack/)
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

- [Run-Length Encoding (RLE)](Run-Length%20Encoding/). Store repeated values as a single byte and a count.
- [Huffman Coding](Huffman%20Coding/). Store more common elements using a smaller number of bits.

### 混淆

- [Shuffle](Shuffle/). Randomly rearranges the contents of an array.
- [Comb Sort](Comb%20Sort/). An improve upon the Bubble Sort algorithm.
- [Convex Hull](Convex%20Hull/).
- [Miller-Rabin Primality Test](Miller-Rabin%20Primality%20Test/). Is the number a prime number?
- [MinimumCoinChange](MinimumCoinChange/). A showcase for dynamic programming.
- [Genetic](Genetic/). A simple example on how to slowly mutate a value to its ideal form, in the context of biological evolution.
- [Myers Difference Algorithm](Myers%20Difference%20Algorithm/). Finding the longest common subsequence of two sequences.

### 数学理论

- [Greatest Common Divisor (GCD)](GCD/). Special bonus: the least common multiple.
- [Permutations and Combinations](Combinatorics/). Get your combinatorics on!
- [Shunting Yard Algorithm](Shunting%20Yard/). Convert infix expressions to postfix.
- [Karatsuba Multiplication](Karatsuba%20Multiplication/). Another take on elementary multiplication.
- [Haversine Distance](HaversineDistance/). Calculating the distance between 2 points from a sphere.
- [Strassen's Multiplication Matrix](Strassen%20Matrix%20Multiplication/). Efficient way to handle matrix multiplication.
- [CounterClockWise](/CounterClockWise/). Determining the area of a simple polygon.

### 机器学习

- [k-Means Clustering](K-Means/). Unsupervised classifier that partitions data into *k* clusters.
- k-Nearest Neighbors
- [Linear Regression](Linear%20Regression/). A technique for creating a model of the relationship between two (or more) variable quantities.
- Logistic Regression
- Neural Networks
- PageRank
- [Naive Bayes Classifier](Naive%20Bayes%20Classifier/)
- [Simulated annealing](Simulated%20annealing/). Probabilistic technique for approximating the global maxima in a (often discrete) large search space.

## 数据结构

The choice of data structure for a particular task depends on a few things.

First, there is the shape of your data and the kinds of operations that you'll need to perform on it. If you want to look up objects by a key you need some kind of dictionary; if your data is hierarchical in nature you want a tree structure of some sort; if your data is sequential you want a stack or queue.

Second, it matters what particular operations you'll be performing most, as certain data structures are optimized for certain actions. For example, if you often need to find the most important object in a collection, then a heap or priority queue is more optimal than a plain array.

Most of the time using just the built-in `Array`, `Dictionary`, and `Set` types is sufficient, but sometimes you may want something more fancy...

### 数组的各种变形

- [Array2D](Array2D/). A two-dimensional array with fixed dimensions. Useful for board games.
- [Bit Set](Bit%20Set/). A fixed-size sequence of *n* bits.
- [Fixed Size Array](Fixed%20Size%20Array/). When you know beforehand how large your data will be, it might be more efficient to use an old-fashioned array with a fixed size.
- [Ordered Array](Ordered%20Array/). An array that is always sorted.
- [Rootish Array Stack](Rootish%20Array%20Stack/). A space and time efficient variation on Swift arrays.

### 队列

- [Stack](Stack/). Last-in, first-out!
- [Queue](Queue/). First-in, first-out!
- [Deque](Deque/). A double-ended queue.
- [Priority Queue](Priority%20Queue). A queue where the most important element is always at the front.
- [Ring Buffer](Ring%20Buffer/). Also known as a circular buffer. An array of a certain size that conceptually wraps around back to the beginning.

### 链表

- [Linked List](Linked%20List/). A sequence of data items connected through links. Covers both singly and doubly linked lists.
- [Skip-List](Skip-List/). Skip List is a probabilistic data-structure with same logarithmic time bound and efficiency as AVL/ or Red-Black tree and provides a clever compromise to efficiently support search and update operations.

### 树

- [Tree](Tree/). A general-purpose tree structure.
- [Binary Tree](Binary%20Tree/). A tree where each node has at most two children.
- [Binary Search Tree (BST)](Binary%20Search%20Tree/). A binary tree that orders its nodes in a way that allows for fast queries.
- [Red-Black Tree](Red-Black%20Tree/). A self balancing binary search tree.
- [Splay Tree](Splay%20Tree/). A self balancing binary search tree that enables fast retrieval of recently updated elements.
- [Threaded Binary Tree](Threaded%20Binary%20Tree/). A binary tree that maintains a few extra variables for cheap and fast in-order traversals.
- [Segment Tree](Segment%20Tree/). Can quickly compute a function over a portion of an array.
  - [Lazy Propagation](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Segment%20Tree/LazyPropagation)
- kd-Tree
- [Sparse Table](Sparse%20Table/). Another take on quickly computing a function over a portion of an array, but this time we'll make it even quicker!.
- [Heap](Heap/). A binary tree stored in an array, so it doesn't use pointers. Makes a great priority queue.
- Fibonacci Heap
- [Trie](Trie/). A special type of tree used to store associative data structures.
- [B-Tree](B-Tree/). A self-balancing search tree, in which nodes can have more than two children.
- [QuadTree](QuadTree/). A tree with 4 children.
- [Octree](Octree/). A tree with 8 children.

### 哈希

- [Hash Table](Hash%20Table/). Allows you to store and retrieve objects by a key. This is how the dictionary type is usually implemented.
- Hash Functions

### 集合

- [Bloom Filter](Bloom%20Filter/). A constant-memory data structure that probabilistically tests whether an element is in a set.
- [Hash Set](Hash%20Set/). A set implemented using a hash table.
- [Multiset](Multiset/). A set where the number of times an element is added matters. (Also known as a bag.)
- [Ordered Set](Ordered%20Set/). A set where the order of items matters.

### 图

- [Graph](Graph/)
- [Breadth-First Search (BFS)](Breadth-First%20Search/)
- [Depth-First Search (DFS)](Depth-First%20Search/)
- [Shortest Path](Shortest%20Path%20%28Unweighted%29/) on an unweighted tree
- [Single-Source Shortest Paths](Single-Source%20Shortest%20Paths%20(Weighted)/)
- [Minimum Spanning Tree](Minimum%20Spanning%20Tree%20%28Unweighted%29/) on an unweighted tree
- [Minimum Spanning Tree](Minimum%20Spanning%20Tree/)
- [All-Pairs Shortest Paths](All-Pairs%20Shortest%20Paths/)
- [Dijkstra's shortest path algorithm](Dijkstra%20Algorithm/)
- [A-Star](A-Star/)

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

Like what you see? Check out [Data Structures & Algorithms in Swift](https://store.raywenderlich.com/products/data-structures-and-algorithms-in-swift), the official book by the Swift Algorithm Club team!

![Data Structures & Algorithms in Swift Book](Images/DataStructuresAndAlgorithmsInSwiftBook.png)

You’ll start with the fundamental structures of linked lists, queues and stacks, and see how to implement them in a highly Swift-like way. Move on to working with various types of trees, including general purpose trees, binary trees, AVL trees, binary search trees, and tries. 

Go beyond bubble and insertion sort with better-performing algorithms, including mergesort, radix sort, heap sort, and quicksort. Learn how to construct directed, non-directed and weighted graphs to represent many real-world models, and traverse graphs and trees efficiently with breadth-first, depth-first, Dijkstra’s and Prim’s algorithms to solve problems such as finding the shortest path or lowest cost in a network.

By the end of this book, you’ll have hands-on experience solving common issues with data structures and algorithms — and you’ll be well on your way to developing your own efficient and useful implementations!

You can find the book on the [raywenderlich.com store](https://store.raywenderlich.com/products/data-structures-and-algorithms-in-swift).

## 鸣谢

Swift 算法俱乐部最初由 [Matthijs Hollemans](https://github.com/hollance) 创建。

现在由 [Vincent Ngo](https://www.raywenderlich.com/u/jomoka)、 [Kelvin Lau](https://github.com/kelvinlauKL) 和 [Richard Ash](https://github.com/richard-ash) 共同维护。

Swift 算法俱乐部是 [raywenderlich.com](https://www.raywenderlich.com) 社区中的[众多算法同好们](https://github.com/raywenderlich/swift-algorithm-club/graphs/contributors)一起努力的成果。期待你的参与 —— [加入俱乐部](.github/CONTRIBUTING.md)？:]

本翻译页面由 [David Lee](https://github.com/davidleee) 创建与维护。

## 许可

All content is licensed under the terms of the MIT open source license.

By posting here, or by submitting any pull request through this forum, you agree that all content you submit or create, both code and text, is subject to this license.  Razeware, LLC, and others will have all the rights described in the license regarding this content.  The precise terms of this license may be found [here](https://github.com/raywenderlich/swift-algorithm-club/blob/master/LICENSE.txt).

[![Build Status](https://travis-ci.org/raywenderlich/swift-algorithm-club.svg?branch=master)](https://travis-ci.org/raywenderlich/swift-algorithm-club)
