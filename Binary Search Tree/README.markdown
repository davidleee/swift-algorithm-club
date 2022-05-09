# 二叉搜索树（BST）

> 这个主题的相关教程在[这里](https://www.raywenderlich.com/139821/swift-algorithm-club-swift-binary-search-tree-data-structure)


二叉搜索树是一种特殊的[二叉树](../Binary%20Tree/)（一种每个节点最多只能有两个子节点的树），它会通过新增或删除节点来让整棵树一直维持有序。

关于树的更多信息，[先看看这里](../Tree/)。

## “一直有序”属性

下面是一个规范的二叉树的例子：

![A binary search tree](Images/Tree1.png)

注意左子节点会保持比右子节点小，并且右子节点会保持比它的父节点要大。这是二叉树的关键特性。

举个例子，`2` 比 `7` 小，所以它去到了左边；`5` 比 `2` 大，所以它去到了右边。

## 插入新节点

在执行插入操作时，我们会先把新的数值与根节点做比较。如果新的数值更小，那我们就往左分支走；如果更大，就往右分支走。我们会一直用这种方式沿着树枝走下去，直到找到一个空的节点，然后就把新数值插入到那里。

假设我们要插入的新数值是 `9`：

- 从树的根节点开始（数字 `7` 所在的节点），与新数值 `9` 做比较。
- `9 > 7`，所以我们在右分支上进行同样的对比，不过这次对比的节点是 `10`。
- 因为 `9 < 10`，所以我们往左分支走。
- 现在我们来到了一个没有数值可以比较的地方。这个新的节点 `9` 就会被插入到这里。

下面是新节点 `9` 插入之后的树：

![After adding 9](Images/Tree2.png)

树上能被插入的位置只有一个。要找到这个位置通常是很快的。它的时间复杂度是 **O(h)**，其中 **h** 表示树的高度。

> **注意：** 节点的*高度*指的是从这个节点走到最深的叶子节点所需要的步数。整棵树的高度就是根节点到最深叶子节点的距离。你会在二叉搜索树上的许多操作里见到“树的高度”这样的术语。

遵循这个简单的规则——较小的数值在左边，较大的数值在右边——我们就能保持树的有序，然后我们就能判断某个数字是否在这棵树里。

## 树的搜索

要在树里找到某个数值，我们的操作步骤跟插入是一样的：

- 如果数值比当前节点小，那就往左分支走。
- 如果数值比当前节点大，那就往右分支走。
- 如果数值与当前节点相等，我们就找到它啦！

就像大多数树的操作一样，这些步骤会递归执行，直到我们找到目标或者已经没有更多的节点可以搜索了。

下面是搜索数字 `5` 的例子：

![Searching the tree](Images/Searching.png)

搜索对树型结构来说是很快的。它的时间复杂度是 **O(h)**。如果你有一个带有100万个节点的已经平衡好的树，它只需要20步就能在树里找到任何一个节点。（这个概念跟数组里的[二分查找](../Binary%20Search)很像。）

## 树的遍历

有时候你需要检查树上所有的节点，而不只是其中一个。

有三种方式遍历一棵树：

1. *中序遍历*（或者叫*深度优先*）：先查看左子节点，然后查看当前节点，最后是右子节点。
2. *前序遍历* ：先查看当前节点，然后才轮到它的左子节点和右子节点。
3. *后序遍历* ：先查看它的左子节点和右子节点，最后才查看当前节点。

遍历一棵树也是基于递归的。

如果你用中序遍历一棵二叉搜索树，那就像是所有节点都已经从小到大排好序让你检查一样。比如遍历例子里的那棵树，它会输出 `1, 2, 5, 7, 9, 10`：

![Traversing the tree](Images/Traversing.png)

## 删除节点

删除节点很简单。删除掉一个节点之后，我们就用它左分支里最大的节点或右分支里最小的节点来替代它原本的位置。这样就能确保树在删除操作后还是有序的。在下面的例子里，10 被删除了，然后被 9 （Figure 2）或者 11 （Figure 3）替代。

![Deleting a node with two children](Images/DeleteTwoChildren.png)

这种替换只在节点有至少一个子节点的时候才需要。如果它没有子节点，你只需要把它从父节点上摘掉就行了。

![Deleting a leaf node](Images/DeleteLeaf.png)


## 代码（方案1）

道理听够了。现在来看看怎么用 Swift 实现一个二叉搜索树。有不同的方案可以选择。首先，我们来看看基于类实现的版本，之后我们还会看看基于枚举的实现方案。

下面是一种 `BinarySearchTree` 类的例子：

```swift
public class BinarySearchTree<T: Comparable> {
  private(set) public var value: T
  private(set) public var parent: BinarySearchTree?
  private(set) public var left: BinarySearchTree?
  private(set) public var right: BinarySearchTree?

  public init(value: T) {
    self.value = value
  }

  public var isRoot: Bool {
    return parent == nil
  }

  public var isLeaf: Bool {
    return left == nil && right == nil
  }

  public var isLeftChild: Bool {
    return parent?.left === self
  }

  public var isRightChild: Bool {
    return parent?.right === self
  }

  public var hasLeftChild: Bool {
    return left != nil
  }

  public var hasRightChild: Bool {
    return right != nil
  }

  public var hasAnyChild: Bool {
    return hasLeftChild || hasRightChild
  }

  public var hasBothChildren: Bool {
    return hasLeftChild && hasRightChild
  }

  public var count: Int {
    return (left?.count ?? 0) + 1 + (right?.count ?? 0)
  }
}
```

这个类只描述了一个节点而不是一棵完整的树。它是一个泛型类型，所以它可以保存任何种类的数据。它还有它左右子节点和父节点的引用。

你可以这样使用它：

```swift
let tree = BinarySearchTree<Int>(value: 7)
```

`count` 属性反映了这颗子树里有多少个节点。这不只是计算它有多少直接子节点，还包括了它子节点的子节点，还有它子节点的子节点的子节点，以此类推。如果这个对象是根节点，它就能算出整棵树一共有多少个节点。初始状态下，`count = 0`。

> **注意：** 因为 `left`、 `right` 和 `parent` 都是可选值，所以我们可以好好用上 Swift 的可选链（`?`）和空值合并运算符（`??`）。你也可以用 `if let` 来实现，但就没那么简洁了。

### 插入节点

只有一个节点的树是没有多大作用的，所以下面展示了如何给树添加新的节点：

```swift
  public func insert(value: T) {
    if value < self.value {
      if let left = left {
        left.insert(value: value)
      } else {
        left = BinarySearchTree(value: value)
        left?.parent = self
      }
    } else {
      if let right = right {
        right.insert(value: value)
      } else {
        right = BinarySearchTree(value: value)
        right?.parent = self
      }
    }
  }
```

就像其他树的操作那样，插入操作用递归来实现是最简单的。我们把新的数值与已经存在于树上的节点进行比较，并且判断应该把它添加到左分支还是右分支里。

如果没有更多的子节点了，我们就为新节点创建一个 `BinarySearchTree` 对象，并通过设置 `parent` 属性把它和这棵树关联起来。

> **注意：** 因为二叉搜索树的关键点就在于把较小的节点放到左边并把较大的节点放到右边，所以你每次都应该从根节点开始插入新节点，以确保这棵二叉树是合法的。

基于上面的例子，你可以这样构建一棵完整的树：

```swift
let tree = BinarySearchTree<Int>(value: 7)
tree.insert(2)
tree.insert(5)
tree.insert(10)
tree.insert(9)
tree.insert(1)
```

> **注意：** 你需要以随机的顺序来插入数字，具体的原因在后面会解释。如果你按数字的大小来插入节点，这棵树的形状就会变得有点奇怪了。

方便起见，我们新增一个初始化方法对一个数组里的所有元素调用 `insert()`：

```swift
  public convenience init(array: [T]) {
    precondition(array.count > 0)
    self.init(value: array.first!)
    for v in array.dropFirst() {
      insert(value: v)
    }
  }
```

现在你就可以这样做了：

```swift
let tree = BinarySearchTree<Int>(array: [7, 2, 5, 10, 9, 1])
```

数组里的第一个数值就成为了树的根节点。

### 调试输出

在使用复杂的数据结构时，易读的调试输出信息会很有帮助。

```swift
extension BinarySearchTree: CustomStringConvertible {
  public var description: String {
    var s = ""
    if let left = left {
      s += "(\(left.description)) <- "
    }
    s += "\(value)"
    if let right = right {
      s += " -> (\(right.description))"
    }
    return s
  }
}
```

当你执行 `print(tree)`，你应该会看到这样的输出

	((1) <- 2 -> (5)) <- 7 -> ((9) <- 10)

根节点在最中间。发挥你的想象力，你应该可以看到像下图这样的树：

![The tree](Images/Tree2.png)

你可能会好奇插入一个重复的元素会怎么样？我们都把重复元素放到右分支里去。试试看吧！

### 搜索

现在我们的树上已经有一些数值，然后呢？当然是搜索啦！二叉搜索树的主要目的就是快速找到元素。:-)

下面是 `search()` 的实现：

```swift
  public func search(value: T) -> BinarySearchTree? {
    if value < self.value {
      return left?.search(value)
    } else if value > self.value {
      return right?.search(value)
    } else {
      return self  // 找到啦!
    }
  }
```

希望这段代码的逻辑是清晰的：从当前节点开始（通常是根节点）进行数值的比较。如果要找的数值比当前节点小，我们就往左分支继续找；如果要找的数值比当前节点大，我们就往右分支里钻。

如果已经没有更多节点可以找了——当 `left` 和 `right` 都是空的——那我们就返回 `nil` 表示要找的数值不在这棵树里。

> **注意：** 在 Swift 里，可选链给上述逻辑提供了不少便利性；当你调用 `left?.search(value)` 的时候，如果 `left` 是空的，它会自动返回 `nil`。我们不需要用 `if` 去明确地检查一下。

搜索是一个递归过程，但你也可以用一个简单的循环去替代它：

```swift
  public func search(_ value: T) -> BinarySearchTree? {
    var node: BinarySearchTree? = self
    while let n = node {
      if value < n.value {
        node = n.left
      } else if value > n.value {
        node = n.right
      } else {
        return node
      }
    }
    return nil
  }
```

确保你清楚知道这两种实现的作用是一样的。我个人更倾向于使用迭代式的代码，但你可以有不同选择。;-)

下面是测试搜索的方法：

```swift
tree.search(5)
tree.search(2)
tree.search(7)
tree.search(6)   // nil
```

前三行会返回对应的 `BinaryTreeNode` 对象。最后一行会返回 `nil` 因为没有一个节点的数值是 `6`。

> **注意：** 如果树里面有重复的元素，`search()` 会返回其中最高的那个节点。这是合理的，因为我们是从根节点开始往下搜索的。

### 遍历

还记得有三种不同的方法可以用来遍历一棵树吗？下面就是例子：

```swift
  public func traverseInOrder(process: (T) -> Void) {
    left?.traverseInOrder(process: process)
    process(value)
    right?.traverseInOrder(process: process)
  }

  public func traversePreOrder(process: (T) -> Void) {
    process(value)
    left?.traversePreOrder(process: process)
    right?.traversePreOrder(process: process)
  }

  public func traversePostOrder(process: (T) -> Void) {
    left?.traversePostOrder(process: process)
    right?.traversePostOrder(process: process)
    process(value)
  }
```

它们的作用是一样的，只是顺序不同。注意所有方法都是基于递归实现的。Swift 的可选链让方法调用更清晰简洁，比如 `traverseInOrder()` 的调用会在没有左或右子节点的时候被忽略。

要从小到大打印出一棵树上的所有数值，你可以这样写：

```swift
tree.traverseInOrder { value in print(value) }
```

输出结果是这个：

	1
	2
	5
	7
	9
	10

你还可以给树添加像 `map()` 和 `filter()` 这样的方法。比如说，下面是一种 `map` 的实现：

```swift
  public func map(formula: (T) -> T) -> [T] {
    var a = [T]()
    if let left = left { a += left.map(formula: formula) }
    a.append(formula(value))
    if let right = right { a += right.map(formula: formula) }
    return a
  }
```

这会对树上的每一个节点调用 `formula` 闭包并收集它们的结果存放到一个数组里。`map()` 是基于中序遍历这颗树的。

一个最简单的 `map()` 的使用方式：

```swift
  public func toArray() -> [T] {
    return map { $0 }
  }
```

这会把树的内容重新转换回一个有序数组。在 playground 里试试看：

```swift
tree.toArray()   // [1, 2, 5, 7, 9, 10]
```

自己实现 `filter` 和 `reduce` 练练手吧。

### 删除节点

我们可以定义一些工具方法来提升代码的可读性：

```swift
  private func reconnectParentTo(node: BinarySearchTree?) {
    if let parent = parent {
      if isLeftChild {
        parent.left = node
      } else {
        parent.right = node
      }
    }
    node?.parent = parent
  }
```

树的改动涉及到许多 `parent`、 `left` 和 `right` 指针的变动。这个方法就是来帮忙做这件事的。它把当前节点——也就是 `self`——的父节点与为另一个节点相连——这个节点会是 `self` 的其中一个子节点。

我们还需要能返回节点最大值和最小值的方法：

```swift
  public func minimum() -> BinarySearchTree {
    var node = self
    while let next = node.left {
      node = next
    }
    return node
  }

  public func maximum() -> BinarySearchTree {
    var node = self
    while let next = node.right {
      node = next
    }
    return node
  }
```

剩下的代码就不需要过多解释了：

```swift
  @discardableResult public func remove() -> BinarySearchTree? {
    let replacement: BinarySearchTree?

    // 用于替换当前节点的可以左分支里的最大值或者右分支里的最小值
    // 只要它不为空就可以
    if let right = right {
      replacement = right.minimum()
    } else if let left = left {
      replacement = left.maximum()
    } else {
      replacement = nil
    }

    replacement?.remove()

    // 把替代者放到当前节点所在的位置
    replacement?.right = right !== replacement ? right : nil
    replacement?.left = left !== replacement ? left : nil
    right?.parent = replacement
    left?.parent = replacement
    reconnectParentTo(node:replacement)

    // 当前节点已经不在树上了，把它清理掉
    parent = nil
    left = nil
    right = nil

    return replacement
  }
```

> **译者注：** 这个方法用到了一个有趣的标记符：`@discardableResult`。在方法有返回值的时候，如果我们不去使用它，那么编译器就会给出一个警告。一种处理办法是用形如 `let _ = method()` 的写法把返回值丢弃，另一种处理办法就是用 `@discardableResult` 把警告屏蔽掉。详情参见 Swift 文档的 [Attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html) 部分。

### 深度和高度

还记得节点的高度表示它到最深叶子节点的距离吗。我们可以用下面的方法来计算高度：

```swift
  public func height() -> Int {
    if isLeaf {
      return 0
    } else {
      return 1 + max(left?.height() ?? 0, right?.height() ?? 0)
    }
  }
```

我们比较左右分支的高度，然后取较大的那个值。同样的，这也是一个递归的过程。因为这个操作需要检查所有子节点，所以复杂度是 **O(n)**。

> **注意：** 我们用 Swift 的空值合并运算符来简化对 `left` 或者 `right` 指针为空时的处理。你也可以用 `if let`，但现在的写法更精确明了。

验证一下：

```swift
tree.height()  // 2
```

你也可以计算出节点的*深度*，也就是它到根节点的距离。下面是示例代码：

```swift
  public func depth() -> Int {
    var node = self
    var edges = 0
    while let parent = node.parent {
      node = parent
      edges += 1
    }
    return edges
  }
```

它会根据 `parent` 指针的信息往树的顶部一步步前进，直到我们来到根节点所在的位置（也就是 `parent` 为空的地方）。这个动作需要花费 **O(h)** 的时间。下面是使用的例子：

```swift
if let node9 = tree.search(9) {
  node9.depth()   // 返回 2
}
```

### 前驱和后继

二叉搜索树永远是“有序”的，但这并不意味着连续的数字会出现在相邻的位置上。

![Example](Images/Tree2.png)

如果你要找一个比 `7` 小的数字，只是查看它的左子节点是不够的。它的左子节点是 `2` 而不是 `5`。同样的道理也适用于寻找比 `7` 大的数字。

`predecessor()` 方法会按顺序返回当前数值的前驱。

```swift
  public func predecessor() -> BinarySearchTree<T>? {
    if let left = left {
      return left.maximum()
    } else {
      var node = self
      while let parent = node.parent {
        if parent.value < value { return parent }
        node = parent
      }
      return nil
    }
  }
```

如果我们有左子树，事情就简单了。这种情况下，直接前驱就是子树的最大值。你可以在上面的例子里得到验证，`5` 就是 `7` 的左子树中的最大值。

如果没有左子树，那我们就要不断往父节点的方向寻找，直到我们找到一个比当前节点更小的数值。如果我们想知道节点 `9` 的前驱节点是哪一个，我们就要不断向上找，直到遇到第一个比自己小的父节点，也就是 `7`。

`successor()` 的代码逻辑是一样的，不过跟上面的代码呈镜像关系：

```swift
  public func successor() -> BinarySearchTree<T>? {
    if let right = right {
      return right.minimum()
    } else {
      var node = self
      while let parent = node.parent {
        if parent.value > value { return parent }
        node = parent
      }
      return nil
    }
  }
```

这两个方法都需要花费 **O(h)** 的时间。

> **注意：** 有一种变体叫 ["线索"二叉树](../Threaded%20Binary%20Tree)，它会把“闲置”的左右指针用于指向这个节点的前驱节点和后继节点。非常聪明的做法！

### 这颗搜索树合法吗？

如果你想搞破坏的话，你可以在一个非根节点上调用 `insert()` 插入一个新节点，这样就会破坏二叉搜索树的合法性。下面是一个例子：

```swift
if let node1 = tree.search(1) {
  node1.insert(100)
}
```

根节点的数值是 `7`，所以一个数值是 `100` 的节点必然会在树的右分支上。然而，上述操作没有从根节点开始执行插入操作，而是在树的左分支的一个叶子节点上执行的。于是，这个新的 `100` 节点就出现在了一个错误的位置上。

这样做的结果是，`tree.search(100)` 会返回空值。

你可以用下面的方法来检查一棵树是不是合法的二叉搜索树：

```swift
  public func isBST(minValue: T, maxValue: T) -> Bool {
    if value < minValue || value > maxValue { return false }
    let leftBST = left?.isBST(minValue: minValue, maxValue: value) ?? true
    let rightBST = right?.isBST(minValue: value, maxValue: maxValue) ?? true
    return leftBST && rightBST
  }
```

这个逻辑检查了左分支里的数值都小于当前节点的数值，并且右分支里的数值都大于当前分支的值。

这个方法可以这样使用：

```swift
if let node1 = tree.search(1) {
  tree.isBST(minValue: Int.min, maxValue: Int.max)  // true
  node1.insert(100)                                 // 要出问题了！！！
  tree.search(100)                                  // nil
  tree.isBST(minValue: Int.min, maxValue: Int.max)  // false
}
```

> **译者注：** 这个例子里假设了根节点是不可能出问题的，所以在第一次调用 `isBST()` 的时候传入了 `Int.min` 和 `Int.max`。这样做无法正确判断根节点的直接子节点就出问题的情况。实际上，这种情况也只会出现在子节点的引用被人为强行修改的时候，而这个类在设计之初就已经禁止了这种行为。

## 代码（方案2）

我们已经基于类实现了二叉搜索树，但你也可以基于枚举来实现。

这两者的区别在于：一个是引用类型，一个是值类型。对基于类实现的树进行改动时，内存里的相同实例也会一起被更新，但基于枚举实现的树却是不可变的——任何插入和删除操作都会给你返回这棵树的全新拷贝。至于哪个更好，完全取决于你要把它用来做什么。

下面是如何构建一棵基于枚举的二叉搜索树：

```swift
public enum BinarySearchTree<T: Comparable> {
  case Empty
  case Leaf(T)
  indirect case Node(BinarySearchTree, T, BinarySearchTree)
}
```

这个枚举包含了三个值：

- `Empty` 标记了分支的结尾（基于类实现的版本是用 `nil` 引用来处理的）。
- `Leaf` 表示一个没有子节点的叶子节点。
- `Node` 表示有一到两个子节点的普通节点。为了能存储 `BinarySearchTree` 类型的数值，这个值被标记成 `indirect` 了。如果没有 `indirect` 标记符，你是不能定义递归型枚举值的。

> **注意：** 这个二叉树上的节点没有它们的父节点的引用。这不是一个大问题，但它会让某些操作的实现更加冗长。

这个实现是递归型的，并且我们需要区别对待不同的枚举值。比如说，你要这样计算节点数量和树的高度：

```swift
  public var count: Int {
    switch self {
    case .Empty: return 0
    case .Leaf: return 1
    case let .Node(left, _, right): return left.count + 1 + right.count
    }
  }

  public var height: Int {
    switch self {
    case .Empty: return -1
    case .Leaf: return 0
    case let .Node(left, _, right): return 1 + max(left.height, right.height)
    }
  }
```

插入新节点是这样做的：

```swift
  public func insert(newValue: T) -> BinarySearchTree {
    switch self {
    case .Empty:
      return .Leaf(newValue)

    case .Leaf(let value):
      if newValue < value {
        return .Node(.Leaf(newValue), value, .Empty)
      } else {
        return .Node(.Empty, value, .Leaf(newValue))
      }

    case .Node(let left, let value, let right):
      if newValue < value {
        return .Node(left.insert(newValue), value, right)
      } else {
        return .Node(left, value, right.insert(newValue))
      }
    }
  }
```

到 playground 里试试：

```swift
var tree = BinarySearchTree.Leaf(7)
tree = tree.insert(2)
tree = tree.insert(5)
tree = tree.insert(10)
tree = tree.insert(9)
tree = tree.insert(1)
```

每次插入操作完成后，你都会获得一个新的树对象，所以你要把结果重新赋值给 `tree` 变量。

下面是至关重要的查询方法：

```swift
  public func search(x: T) -> BinarySearchTree? {
    switch self {
    case .Empty:
      return nil
    case .Leaf(let y):
      return (x == y) ? self : nil
    case let .Node(left, y, right):
      if x < y {
        return left.search(x)
      } else if y < x {
        return right.search(x)
      } else {
        return self
      }
    }
  }
```

这些方法大都有着相同的结构。

到 playground 里试试：

```swift
tree.search(10)
tree.search(1)
tree.search(11)   // nil
```

为了在调试时打印出整棵树，你可以用这个方法：

```swift
extension BinarySearchTree: CustomDebugStringConvertible {
  public var debugDescription: String {
    switch self {
    case .Empty: return "."
    case .Leaf(let value): return "\(value)"
    case .Node(let left, let value, let right):
      return "(\(left.debugDescription) <- \(value) -> \(right.debugDescription))"
    }
  }
}
```

当你执行 `print(tree)`，结果会长这样：

	((1 <- 2 -> 5) <- 7 -> (9 <- 10 -> .))

根节点在中间，圆点表示这个位置没有子节点。

## 当树失去平衡时...

我们说一棵二叉搜索树是*平衡*时，说明它的左右子树都有着相同数量的节点。这种情况下，树的高度是 *log(n)*，其中 *n* 表示节点的个数。这是最理想的状况。

如果其中一个分支比另一个长得多，搜索就会变得非常慢。因为我们要判断的数值比本来需要的多得多。在最坏情况下，树的高度可能会是 *n*。这样的一棵树已经和一个[链表](../Linked%20List/)没什么两样，而不是一棵二叉搜索树了，它的搜索性能也会下降到 **O(n)**。太糟糕了！

其中一种让树平衡的方法是以完全随机的顺序插入节点。平均来说，这应该能给树提供还不错的平衡性，但这不能保证成功，而且这个方法也不总是满足实际情况。

另一种解决方法是使用一棵*自平衡*的二叉树。这类数据结构会在你插入或删除节点后重新调整树的平衡。举个例子，你可以看看[AVL 树](../AVL%20Tree) 和 [红黑树](../Red-Black%20Tree)。

## 相关信息

[维基百科上的搜索二叉树](https://en.wikipedia.org/wiki/Binary_search_tree)

*原文出自 [Nicolas Ameghino](http://www.github.com/nameghino) 和 Matthijs Hollemans*
