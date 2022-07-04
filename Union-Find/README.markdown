# 并查集

当一组元素被分隔成若干个不相交的子集时，并查集就是能用于跟踪这些不重合子集的数据结构。它也被称为不相交集合。

这个绕口的定义是什么意思呢？举个例子，并查集可以用来跟踪下面这样的集合：

	[ a, b, f, k ]
	[ e ]
	[ g, d, c ]
	[ i, j ]

这些集合是 **不相交的** 因为它们之间不存在相同的元素。

并查集支持下面三种基础操作：

1. **Find(A)**：判断元素 **A** 在哪一个子集里。比如说，`find(d)` 会返回子集 `[ g, d, c ]`。

2. **Union(A, B)**：把包含元素 **A** 和 **B** 的两个子集合并成一个。比如说，`union(d, j)` 会把子集 `[ g, d, c ]` 和 `[ i, j ]` 合并成一个更大的集合 `[ g, d, c, i, j ]`。

3. **AddSet(A)**：添加一个只包含了元素 **A** 的新子集。比如说，`addSet(h)` 会添加一个新的集合 `[ h ]`。

这种数据结构最常用于跟踪无向[图](../Graph/)里的相连组件。它还经常被用于实现 Kruskal 算法的另一种更高效版本，这种算法会被用来查找图的最小生成树。

## 实现

并查集有许多种实现方式，我们来看一种高效而且易于理解的方案：加权快速并集。
> __PS: 文章附带的 playground 里有多种并查集的实现方式。__

```swift
public struct UnionFind<T: Hashable> {
  private var index = [T: Int]()
  private var parent = [Int]()
  private var size = [Int]()
}
```

我们的并查集其实是一座森林，其中每一个子集都用一颗[树](../Tree/)来表示。

基于我们要实现的目标，我们只需要跟踪树上每个节点的父节点，而不需要跟踪它们的子节点。于是我们用一个 `parent` 数组来管理父节点，其中 `parent[i]` 表示节点 `i` 的父节点的下标。

例子：如果 `parent` 长这样：

	parent [ 1, 1, 1, 0, 2, 0, 6, 6, 6 ]
	     i   0  1  2  3  4  5  6  7  8

那么对应的树结构就长这样：

	      1              6
	    /   \           / \
	  0       2        7   8
	 / \     /
	3   5   4

这是森林里的两棵树，每一棵树都对应了一组元素。（小提醒：基于上面这种绘图方式的限制，两棵树都被画成了二叉树，但实际上它们不一定要是这种形式。）

我们把每一个子集都用一个唯一的数字来标记。这个用来标记的数字就是子集树的根节点的下标。在这个例子里，节点 `1` 是第一个棵树的根节点，而 `6` 是第二棵树的根节点。

所以在这个例子中我们有两个子集，第一个子集会用 `1` 来标记，而第二个则用 `6` 来标记。前面提到的 **Find** 方法其实返回的是集合的标记数字，而不是它的内容。

注意根节点的 `parent[]` 指向的是它自己。所以有 `parent[1] = 1` 和 `parent[6] = 6`。这也是我们用来判断根节点的方法。

## 添加集合

来看看这些基础操作的实现，从添加一个新的集合开始。

```swift
public mutating func addSetWith(_ element: T) {
  index[element] = parent.count  // 1
  parent.append(parent.count)    // 2
  size.append(1)                 // 3
}
```

当你添加一个新的元素时，其实是添加了一个只包含了这个新元素的新子集。

1. 我们把新元素的下标保存到 `index` 字典里。这样做能让未来查找元素的操作更快一些。

2. 然后我们把这个下标添加到 `parent` 数组里，为这个集合构建一棵新的树。这个时候，`parent[i]` 指向的正是它自己，因为这棵树所表示的集合只有一个元素，所以这个元素表示的节点当然就是树的根节点。

3. `size[i]` 表示根节点在下标 `i` 的树所包含的节点个数。对于新集合来说，因为它只有一个元素，所以这里保存的就是 1。我们会在求交集的操作里用到 `size` 数组。

## 查找

我们时常需要判断某个元素是不是已经存在于某个集合里。这时候就轮到 **Find** 操作出马了。在我们的 `UnionFind` 数据结构里，这个操作叫 `setOf()`：

```swift
public mutating func setOf(_ element: T) -> Int? {
  if let indexOfElement = index[element] {
    return setByIndex(indexOfElement)
  } else {
    return nil
  }
}
```

它会在 `index` 字典里找到元素的下标，然后通过一个工具方法来找到这个元素所属的集合：

```swift
private mutating func setByIndex(_ index: Int) -> Int {
  if parent[index] == index {  // 1
    return index
  } else {
    parent[index] = setByIndex(parent[index])  // 2
    return parent[index]       // 3
  }
}
```

因为我们在处理一个树结构，所以这里用到的是一个递归方法。

上文有提到每个集合都是由一棵树来表示的，并且根节点的下标会被用来标识这个集合。我们要找的是目标元素所属的树的根节点，然后返回它的下标。

1. 首先，我们会检查传入的下标是不是表示了一个根节点（也就是节点中的 `parent` 指向了自己的）。如果是这样，那我们就搞定收工了。

2. 否则，我们就对当前节点的父节点递归调用这个方法。然后我们要做一件 **非常重要的事情**：用根节点覆盖掉当前节点的父节点，这会把当前节点直接与树的根节点相连。下次我们再调用这个方法的时候，因为到树根的路径变短了，所以它会执行得更快。如果没有这层优化，这个方法的复杂度是 **O(n)**，但现在有了这层优化之后，复杂度就几乎是 **O(1)**。

3. 我们把根节点的下标作为结果返回出去。

下面来演示一下整个过程。假如有这样的一棵树：

![BeforeFind](Images/BeforeFind.png)

我们执行 `setOf(4)`。为了找到根节点，我们首先要去往节点 `2`，然后是节点 `7`。（元素的索引用红色表示）

在执行 `setOf(4)` 的过程中，这棵树被重新调整成了这样：

![AfterFind](Images/AfterFind.png)

现在如果再次调用 `setOf(4)`，我们就不用经过 `2` 这个节点才能到达根节点。于是，并查集这种数据结构就会在被使用的过程中自行优化自己的尺寸。厉害啊！

还有一个工具方法可以用来判断两个元素是否在同一个集合里：

```swift
public mutating func inSameSet(_ firstElement: T, and secondElement: T) -> Bool {
  if let firstSet = setOf(firstElement), let secondSet = setOf(secondElement) {
    return firstSet == secondSet
  } else {
    return false
  }
}
```

这个方法也调用了 `setOf()` 所以它也会对树进行优化。

## 并集 (加权)

最后一个操作是 **并集**，它会将两个集合合并成一个更大的集合。

```swift
public mutating func unionSetsContaining(_ firstElement: T, and secondElement: T) {
    if let firstSet = setOf(firstElement), let secondSet = setOf(secondElement) { // 1
        if firstSet != secondSet {                // 2
            if size[firstSet] < size[secondSet] { // 3
                parent[firstSet] = secondSet      // 4
                size[secondSet] += size[firstSet] // 5
            } else {
                parent[secondSet] = firstSet
                size[firstSet] += size[secondSet]
            }
        }
    }
}
```

下面是它的工作原理：

1. 我们找到每个元素所属的结合。这会给我们返回两个整数：分别表示 `parent` 数组里各自的根节点的索引。

2. 检查这两个集合是不是同一个，因为如果它们在同一个集合里，就没有必要去合并什么了。

3. 这是尺寸优化发挥作用的地方（添加权重）。我们希望尽可能让树更矮，所以我们总是把最小的树连接到最大的树的根节点上去。我们通过比较树的尺寸来判断哪一棵树是更小的。

4. 这里我们把比较小的那棵树连接到比较大的那棵树的根节点上。

5. 因为我们刚刚把新节点添加到了较大树上，所以要更新它的尺寸。

下面的示例也许能帮助理解这个过程。假设我们有如下两个集合，每个集合都由各自的树来表示：

![BeforeUnion](Images/BeforeUnion.png)

现在我们来调用 `unionSetsContaining(4, and: 3)` 方法。较小的树会被添加到较大的树上：

![AfterUnion](Images/AfterUnion.png)

注意看，因为我们在方法的一开始就调用了 `setOf()`，所以较大树在过程中也会被优化 —— 节点 `3` 现在直接连接到了根节点上。

优化后的并集过程也几乎有着 **O(1)** 的复杂度。

## 路径压缩
```swift
private mutating func setByIndex(_ index: Int) -> Int {
    if index != parent[index] {
        // Updating parent index while looking up the index of parent.
        parent[index] = setByIndex(parent[index])
    }
    return parent[index]
}
```
路经压缩能帮助树维持扁平，于是查询操作的复杂度就  **几乎** 是 __O(1)__。

## 复杂度总结

##### 处理 N 个对象
| 数据结构 | 并集 | 查找 |
|---|---|---|
|快速查询|N|1|
|快速并集|树的高度|树的高度|
|加权快速并集|lgN|lgN|
|加权快速并集 + 路径压缩| 非常接近但达不到 O(1)| 非常接近但达不到 O(1) |

##### 对 N 个对象执行 M 次并集操作
| 算法 | 最坏耗时 |
|---|---|
|快速查询| M N |
|快速并集| M N |
|加权快速并集| N + M lgN |
|加权快速并集 + 路径压缩| (M + N) lgN |

## 相关信息

在 playground 里有更多关于如何使用这个方便的数据结构的例子。

[维基百科上的并查集](https://en.wikipedia.org/wiki/Disjoint-set_data_structure)

*原文出自 [Artur Antonov](https://github.com/goingreen)* *经由 [Yi Ding](https://github.com/antonio081014) 编辑*