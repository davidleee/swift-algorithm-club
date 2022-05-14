# 归并排序

> 这个主题的相关教程在[这里](https://www.raywenderlich.com/154256/swift-algorithm-club-swift-merge-sort)。

目标：从低到高给一个数组排序（或者从高到低）。

John von Neumann 在 1945 年发明了归并排序，它是一种最佳、最坏和平均情况都是 **O(n log n)** 复杂度的高效算法。

归并排序算法使用了分治法，这是一种先把大问题分解成若干小问题再解决它们的方法。我认为归并排序算法可以简单分为两个步骤：**先拆分** 和 **后合并**。

假设你要把由 *n* 个数字组成的数组排成合适的顺序。归并排序算法会是这样做的：

- 把数字先无序地放到一堆
- 把这堆数字一分为二。现在你就有 **两堆无序的** 数字了。
- 继续拆分这些数字堆，直到你再也分不下去了。最后你会有 *n* 堆数字，每堆里面只有一个数字。
- 现在开始通过把它们按顺序结对的方式 **合并** 它们。每次合并的时候，要把内容有序排放。这个动作其实很简单，因为每堆数字本身已经排好序了。

## 一个例子

### 拆分

假设你有一个数字组成的数组 `[2, 1, 5, 4, 9]`。这是一堆无序的数字。现在的目标是把它拆分到再也拆不下去为止。

首先，把数组分成两半：`[2, 1]` 和 `[5, 4, 9]`。还能继续拆分吗？当然可以啦！

把注意力放到左边这堆。把 `[2, 1]` 拆成 `[2]` 和 `[1]`。还能继续拆分吗？不行了。该看看另一堆数字了。

把 `[5, 4, 9]` 拆成 `[5]` 和 `[4, 9]`。不出意料，`[5]` 已经不能再分了，但是 `[4, 9]` 还能被分成 `[4]` 和 `[9]`。

拆分的过程最后得到了这堆数字：`[2]` `[1]` `[5]` `[4]` `[9]`。可以看到每堆数字都只有一个元素。

### 合并

既然你已经把数组拆分了，现在就要一边 **排序** 一边 **合并** 它们了。记住，主旨是要解决许多个小问题而不是一个大问题。在每一次合并的迭代里，你需要考虑的都是怎么把一堆数字和另一堆合起来。

现在你的数字堆是 `[2]` `[1]` `[5]` `[4]` `[9]`，第一轮合并过后的结果是 `[1, 2]` 和 `[4, 5]` 和 `[9]`。因为 `[9]` 被剔除在外了，所以在这一轮里你没法把它跟其他数字合并了。

下一轮会把`[1, 2]` 和 `[4, 5]` 合并起来。结果是 `[1, 2, 4, 5]` 和再次被单独出来的 `[9]`。

于是你只剩下两堆数字了：`[1, 2, 4, 5]` 和 `[9]`，终于有机会合并了，最终有序的数组是 `[1, 2, 4, 5, 9]`。

## 自顶向下的实现

下面是 Swift 里的归并排序的样子：

```swift
func mergeSort(_ array: [Int]) -> [Int] {
  guard array.count > 1 else { return array }    // 1

  let middleIndex = array.count / 2              // 2

  let leftArray = mergeSort(Array(array[0..<middleIndex]))             // 3

  let rightArray = mergeSort(Array(array[middleIndex..<array.count]))  // 4

  return merge(leftPile: leftArray, rightPile: rightArray)             // 5
}
```

一步步解释这段代码是干嘛的：

1. 如果数组是空的或者只有一个元素，那就没办法把它拆分成更小的部分了。你只好直接返回这个数组了。

2. 找到最中间的下标

3. 用上一步的中间下标，递归地拆分左边的数组。

4. 并且，递归地拆分右边的数组。

5. 最后，把所有数值合并到一起，确保它们是有序的。

下面是合并的算法：

```swift
func merge(leftPile: [Int], rightPile: [Int]) -> [Int] {
  // 1
  var leftIndex = 0
  var rightIndex = 0

  // 2
  var orderedPile = [Int]()
  orderedPile.reserveCapacity(leftPile.count + rightPile.count)

  // 3
  while leftIndex < leftPile.count && rightIndex < rightPile.count {
    if leftPile[leftIndex] < rightPile[rightIndex] {
      orderedPile.append(leftPile[leftIndex])
      leftIndex += 1
    } else if leftPile[leftIndex] > rightPile[rightIndex] {
      orderedPile.append(rightPile[rightIndex])
      rightIndex += 1
    } else {
      orderedPile.append(leftPile[leftIndex])
      leftIndex += 1
      orderedPile.append(rightPile[rightIndex])
      rightIndex += 1
    }
  }

  // 4
  while leftIndex < leftPile.count {
    orderedPile.append(leftPile[leftIndex])
    leftIndex += 1
  }

  while rightIndex < rightPile.count {
    orderedPile.append(rightPile[rightIndex])
    rightIndex += 1
  }

  return orderedPile
}
```

这个方法可能看起来有点可怕，但其实它是挺直白的：

1. 你需要用两个下标来跟踪两个数组的合并进度。

2. 这个是合并后的数组。它现在还是空的，但你会一步步把其他数组的元素添加到它里面去。既然你已经知道这个数组最后会有多少个元素了，那你就可以通过保留容量来避免后续的内存重新分配了。

3. 这个 while 循环会比较左右两边的元素并把它们按顺序添加到 `orderPile` 里面去。

4. 如果上一个 while 循环结束了，那就意味着 `leftPile` 或者 `rightPile` 里的内容被全部合并到了 `orderPile` 里去。到了这个时候，你就不需要再进行比较了。直接把剩下的内容一股脑全部转移过去就可以了。

来看一个 `merge()` 运行的例子，假设我们有这些数字堆：`leftPile = [1, 7, 8]` 和 `rightPile = [3, 6, 9]`。注意这两堆数字都分别是有序的 —— 这一点在归并排序里是可以得到保证的。下面是把数组合并成一堆更大的数组的步骤：

	leftPile       rightPile       orderedPile
	[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ ]
      l              r

左边的下标用 `l` 来表示，它指向了左边数组里的第一个元素。右边的下标用 `r` 表示，它指向了 `3`。于是，第一个被添加到 `orderedPile` 里的元素就是 `1`。我们还要把下标 `l` 移动到下一个元素上。

	leftPile       rightPile       orderedPile
	[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1 ]
      -->l           r

现在 `l` 指向了 `7` 但 `r` 还是 `3`。我们把最小的元素放进有序堆，也就是 `3`。情况现在变成：

	leftPile       rightPile       orderedPile
	[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3 ]
         l           -->r

这个过程不断重复。在每一步里，我们从 `leftPile` 或 `rightPile` 里选择最小的元素并添加到 `orderedPile` 里：

	leftPile       rightPile       orderedPile
	[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3, 6 ]
         l              -->r

	leftPile       rightPile       orderedPile
	[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3, 6, 7 ]
         -->l              r

	leftPile       rightPile       orderedPile
	[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3, 6, 7, 8 ]
            -->l           r

好了，左边的数组里已经没有元素了。我们直接把右边数组里剩下的元素都添加上去，就完成了。合并后的数组是 `[ 1, 3, 6, 7, 8, 9 ]`。

这个算法其实很简单：它从左到右遍历两堆数字，并且每一步都会选出最小的元素。这样做能成功是因为我们已经确保了每堆数字本身已经是有序的了。

## 自底向上的实现

刚才你看到的归并排序算法实现被称为“自顶向下”的，因为它是先拆分数组再把它们合并起来的。其实在对一个数组进行排序的时候，你可以跳过拆分的步骤，直接开始合并数组里的元素（链表就不能这样）。这被称为“自底向上”的实现方式。

是时候加速一下了。:-) 下面是用 Swift 自底向上实现归并排序的完整方法：

```swift
func mergeSortBottomUp<T>(_ a: [T], _ isOrderedBefore: (T, T) -> Bool) -> [T] {
  let n = a.count

  var z = [a, a]      // 1
  var d = 0

  var width = 1
  while width < n {   // 2

    var i = 0
    while i < n {     // 3

      var j = i
      var l = i
      var r = i + width

      let lmax = min(l + width, n)
      let rmax = min(r + width, n)

      while l < lmax && r < rmax {                // 4
        if isOrderedBefore(z[d][l], z[d][r]) {
          z[1 - d][j] = z[d][l]
          l += 1
        } else {
          z[1 - d][j] = z[d][r]
          r += 1
        }
        j += 1
      }
      while l < lmax {
        z[1 - d][j] = z[d][l]
        j += 1
        l += 1
      }
      while r < rmax {
        z[1 - d][j] = z[d][r]
        j += 1
        r += 1
      }

      i += width*2
    }

    width *= 2
    d = 1 - d      // 5
  }
  return z[d]
}
```

这看起来比自顶向下的版本要恐怖多了，但是仔细看，方法的主体还是 `merge()` 里的三个 `while` 循环。

要注意的点：

1. 归并排序算法需要一个临时的工作数组，因为你不能在合并左右两堆数字的同时改变它们的内容。因为为每次合并重新分配一个数组会很浪费，所以我们用两个工作数组，并通过 `d` 的数值变化来切换它们，也就是 0 或者 1。数组 `z[d]` 是用来读取的，而数组 `z[1-d]` 是用来写入的。这种方式叫做*双缓冲*。

2. 在概念上，自底向上的版本和自顶向下的版本是同样的逻辑。首先它会把元素一个个合并，然后它会把它们两个两个合并，接下来是四个四个合并，以此类推。数字堆的大小是通过 `width` 来判断的。一开始，`width` 是 1，但在每次循环的结尾，我们都会把它乘以二，所以这个外层循环决定了要合并的数字堆的大小，并且即将被用来合并的子数组在每次循环后都会变得更长。

3. 内层循环会遍历每个数字堆，并且会把每一对数字堆合并成更大的一堆数字。合并的结果会写进 `z[1 - d]` 指向的那个数组里。

4. 这跟自顶向下版本的逻辑是一样的。主要的区别在于我们使用了双缓冲，所以数值会从 `z[d]` 读取并写入到 `z[1 - d]`。它还用了一个 `isOrderedBefore` 方法来比较元素，而不是单纯的 `<`，于是这个版本的归并排序就更普遍了，你可以把它用在任何类型对象的排序上。

5. 来到这里的时候，数组 `z[d]` 里长度为 `width` 的数组堆已经被合并成了长度为 `width * 2` 的更大的一堆，并写入到了数组 `z[1 - d]` 里去。在这里，我们会交换工作数组，所以下次循环就会从我们刚处理好的一堆新数字里读取数据。

这个方法是通用的，你可以把它用于任意类型数据的排序，只要你提供一个合适的 `isOrderedBefore` 闭包来比较元素就可以了。

使用例子：

```swift
let array = [2, 1, 5, 4, 9]
mergeSortBottomUp(array, <)   // [1, 2, 4, 5, 9]
```

## 性能

归并排序算法的速度取决于待排序的数组长度。数组越长，它要的工作量就越大。

初始数组是否有序并不会影响归并排序算法的速度，因为你总是需要处理相同数量的拆分和比较，而不会考虑初始元素的顺序。

因此，时间复杂度在最佳、最坏和平均情况下都一直是 **O(n log n)**。

归并排序算法的一个缺点是，它需要一个长度与待排序数组一致的临时“工作”数组。它不是 **就地** 排序，这一点与[快速排序](../Quicksort/)不一样。

> **译者注：** ”就地“（in-place）排序是指不需要使用额外内存的排序，归并排序属于“非就地“（out-place）排序。

大多数归并排序算法的实现都是 **稳定的**。这表示有着相同排序关键字的数组元素，在排序之后依然会保持它们原本的相对位置。这对于简单数值，比如数字和字符串，来说无关紧要，但是在给复杂对象排序时可能需要注意。

## 相关信息

[维基百科上的归并排序](https://en.wikipedia.org/wiki/Merge_sort)

*原文出自 Kelvin Lau。由 Matthijs Hollemans 进行补充。*