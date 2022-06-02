# 第 k 大元素问题

你现在有一个整型数组 `a`。实现一个算法用于找出数组中第 *k* 大的元素。

举个例子，第 1 大的元素就是数组里出现的最大值。如果数组里有 *n* 个元素，那么第 *n* 大的元素就是最小值。它的中位数就是第 *n/2* 大的元素。

## 简单直白解法

下面的解法还算比较直白。它的时间复杂度是 **O(n log n)** 因为它首先对数组进行了排序，并且还使用了额外的 **O(n)** 大小的空间。

```swift
func kthLargest(a: [Int], k: Int) -> Int? {
  let len = a.count
  if k > 0 && k <= len {
    let sorted = a.sorted()
    return sorted[len - k]
  } else {
    return nil
  }
}
```

这个 `kthLargest()` 函数需要两个参数：由整型组成的数组 `a`，和 `k`。它会返回第 *k* 大的元素。

现在跟着示例，模拟运行一遍算法的流程来看看它是怎么运作的。假设 `k = 4` 并且数组如下：

```swift
[ 7, 92, 23, 9, -1, 0, 11, 6 ]
```

起初我们并没有直接的手段去寻找第 k 大的元素，但是在给数组排完序之后事情就变得简单直接的。下面是排序后的数组：

```swift
[ -1, 0, 6, 7, 9, 11, 23, 92 ]
```

现在，我们需要做的只是把下标为 `a.count - k` 的数值取出来：

```swift
a[a.count - k] = a[8 - 4] = a[4] = 9
```

当然了，如果你要找的是第 k **小** 的元素，那你就要取 `a[k - 1]`。

## 一种更快的解法

有一种更聪明的解法，它结合了[二分查找](../Binary%20Search/)和[快速排序](../Quicksort/)的思路，实现了一种复杂度为 **O(n)** 的解决方案。

回忆一下二分查找的工作原理：把数组一次又一次地对半分开，以快速缩小查询目标的范围。这也是我们这次要用的方法。

快速排序也会拆分数组。它会将比关键点小的数值移动到它的左边并把比它大的数值移动到右边。在这样使用关键点分隔数组之后，这个点的数值就已经位于它的最终的、排好序后的位置了。我们在这里也可以利用这个方法的优势。

这是它的工作原理：我们随机选择一个关键点，围绕它把数组分隔开，然后只在左区间或右区间里进行类似二分查找的操作。不断重复这个操作，直到找到某个恰好在第 *k* 位的关键点。

让我们回到一开始的例子里。我们要找数组里的第 4 大元素：

	[ 7, 92, 23, 9, -1, 0, 11, 6 ]

这个算法在找第 k **小** 的元素时会更容易理解，所以我们假设 `k = 4` 并寻找第 4 小的元素。

注意我们不需要先对数组进行排序了。我们随机选择一个元素作为关键点，假设是 `11` 吧，并把数组围绕它进行分隔。我们可能会得到这样的结果：

	[ 7, 9, -1, 0, 6, 11, 92, 23 ]
	 <------ 较小        较大 -->

如你所见，所有比 `11` 小的数值都在左边；所有比它大的数值都在右边。关键点 `11` 现在已经在它的最终位置了。关键点的下标是 5，所以第 4 小的元素肯定在左边区间里的某个地方。于是我们从现在开始可以忽略数组的其他部分了：

	[ 7, 9, -1, 0, 6, x, x, x ]

再来随机选择一个关键点，假设是 `6` 吧，并把数组围绕它进行分隔。我们可能会得到这样的结果：

	[ -1, 0, 6, 9, 7, x, x, x ]

关键点 `6` 最后停在了下标 2 的位置，所以显然第 4 小的元素肯定在右边的区间里。我们可以忽略左边的区间了：

	[ x, x, x, 9, 7, x, x, x ]

我们再选一个随机的关键点，假设是 `9` 吧，并对数组进行分隔：

	[ x, x, x, 7, 9, x, x, x ]

关键点 `9` 的下标是 4，而这正是我们要找的 *k*。我们完事儿啦！留意到了吗？刚刚的过程只花了很少的步骤，而且我们也不用先对数组进行排序了。

下面的函数实现了这个概念：

```swift
public func randomizedSelect<T: Comparable>(_ array: [T], order k: Int) -> T {
  var a = array

  func randomPivot<T: Comparable>(_ a: inout [T], _ low: Int, _ high: Int) -> T {
    let pivotIndex = random(min: low, max: high)
    a.swapAt(pivotIndex, high)
    return a[high]
  }

  func randomizedPartition<T: Comparable>(_ a: inout [T], _ low: Int, _ high: Int) -> Int {
    let pivot = randomPivot(&a, low, high)
    var i = low
    for j in low..<high {
      if a[j] <= pivot {
        a.swapAt(i, j)
        i += 1
      }
    }
    a.swapAt(i, high)
    return i
  }

  func randomizedSelect<T: Comparable>(_ a: inout [T], _ low: Int, _ high: Int, _ k: Int) -> T {
    if low < high {
      let p = randomizedPartition(&a, low, high)
      if k == p {
        return a[p]
      } else if k < p {
        return randomizedSelect(&a, low, p - 1, k)
      } else {
        return randomizedSelect(&a, p + 1, high, k)
      }
    } else {
      return a[low]
    }
  }

  precondition(a.count > 0)
  return randomizedSelect(&a, 0, a.count - 1, k)
}
```

为了增加可读性，整个功能被分成了三个内部函数：

- `randomPivot()` 会随机选出一个数组并把它放到当前区间的尾部（这是使用 Lomuto 分隔法的必要条件，详情可见于[快速排序](../Quicksort/)里的讨论）。

- `randomizedPartition()` 就是快速排序里的 Lomuto 分隔法。在它运行完之后，那个随机选出的关键点就会停在有序数组里的最终位置。它会返回关键点的下标。

- `randomizedSelect()` 负责所有的苦力活。它会先调用分隔函数，然后再决定接下来要做什么。如果关键点的下标等于我们要找的 *k*，那我们就完成了。如果 *k* 比关键点下标要小，那它肯定在左边的区间，于是我们会在那边继续递归地尝试查找。同样的操作也适用于 *k* 肯定在右边区间的情况。

厉害吧？快速排序通常是一个 **O(n log n)** 的算法，但因为我们要分隔的区间只会越来越小，所以 `randomizedSelect()` 的运行时间会是 **O(n)**。

> **注意：** 这个函数计算的是数组里第 *k* 小的元素，且 *k* 是从 0 开始的。如果你要找第 *k* 大的元素，那你要用 `a.count - k` 作为参数调用它。

*原文出自 Daniel Speiser。由 Matthijs Hollemans 进行补充。*
