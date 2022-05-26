# 查找最小值/最大值

目标：在一个无序的数组中找出最小/最大的对象。

## 最大值 或 最小值


我们有一个由普通对象构成的数组，并且在遍历它所有元素的过程中记下当前时刻找到的最小/最大的元素。

### 一个例子

比方说我们要找出 `[ 8, 3, 9, 4, 6 ]` 这个无序列表里的最大值。

拿出第一个数字，`8`，并把它记录为当前时刻的最大元素。

从列表里拿出下一个数字，`3`，并把它和当前时刻的最大值做比较。`3` 比 `8` 要小，所以 `8` 还是作为最大值不用改变。

从列表里拿出下一个数字，`9`，并把它和当前时刻的最大值做比较。`9` 比 `8` 要大，所以我们把 `9` 记录为当前时刻的最大值。

重复这个过程，知道列表里的所有元素都被处理过了。

### 代码

下面是基于 Swift 的简单实现：

```swift
func minimum<T: Comparable>(_ array: [T]) -> T? {
  guard var minimum = array.first else {
    return nil
  }

  for element in array.dropFirst() {
    minimum = element < minimum ? element : minimum
  }
  return minimum
}

func maximum<T: Comparable>(_ array: [T]) -> T? {
  guard var maximum = array.first else {
    return nil
  }

  for element in array.dropFirst() {
    maximum = element > maximum ? element : maximum
  }
  return maximum
}
```

把这段代码放进 playground 就可以这样验证它：

```swift
let array = [ 8, 3, 9, 4, 6 ]
minimum(array)   // 这会返回 3
maximum(array)   // 这会返回 9
```

### 在 Swift 的标准库里

Swift 标准库里已经对 `SequenceType` 做了扩展，能够返回一系列元素里最大/最小的那个。

```swift
let array = [ 8, 3, 9, 4, 6 ]
array.minElement()   // 这会返回 3
array.maxElement()   // 这会返回 9
```

```swift
let array = [ 8, 3, 9, 4, 6 ]
//swift3
array.min()   // 这会返回 3
array.max()   // 这会返回 9
```

## 最大值 和 最小值

为了同时找到数组里的最大值和最小值，并尽量减少给数字做比较的次数，我们可以成对地比较元素。

### 一个例子

假设我们要找出 `[ 8, 3, 9, 4, 6 ]` 这个无序列表里的最大值和最小值。

拿出第一个数字，`8`，并把它记录为当前时刻的最大元素和最小元素。

因为我们列表里的元素个数是奇数，所以在我们把 `8` 移出列表之后，就剩下了两对元素：`[ 3, 9 ]` 和 `[ 6, 4 ]`。

拿出列表里的下一对数字，`[ 3, 9 ]`。在这两个数字当中，`3` 是比较小的那一个，所以我们把 `3` 跟当前的最小值 `8` 进行比较，然后把 `9` 和当前的最大值 `8` 进行比较。`3` 比 `8` 小，所以新的最小值是 `3`。`9` 比 `8` 大，所以新的最大值是 `9`。

拿出列表里的下一对数字，`[ 6, 4 ]`。这一次，`4` 是比较小的那个，所以把 `4` 和当前最小值 `3` 比较，并且把 `6` 和当前最大值 `9` 比较。`4` 比 `3` 大，所以最小值不变。`6` 比 `9` 小，所以最大值不变。

最终的结果是，最小值为 `3` 和 最大值为`9`。

### 代码

下面是基于 Swift 的简单实现：

```swift
func minimumMaximum<T: Comparable>(_ array: [T]) -> (minimum: T, maximum: T)? {
  guard var minimum = array.first else {
    return nil
  }
  var maximum = minimum

  // 如果 `array` 有奇数个元素，那就让 'minimum' 或 'maximum' 与多出来的那一个成对
  let start = array.count % 2 // 如果是奇数就会返回1，表示跳过第一个元素
  for i in stride(from: start, to: array.count, by: 2) {
    let pair = (array[i], array[i+1])

    if pair.0 > pair.1 {
      if pair.0 > maximum {
        maximum = pair.0
      }
      if pair.1 < minimum {
        minimum = pair.1
      }
    } else {
      if pair.1 > maximum {
        maximum = pair.1
      }
      if pair.0 < minimum {
        minimum = pair.0
      }
    }
  }

  return (minimum, maximum)
}
```

把这段代码放进 playground 就可以这样验证它：

```swift
let result = minimumMaximum(array)!
result.minimum   // 这会返回 3
result.maximum   // 这会返回 9
```

通过成对取出元素并把它们分别跟最大值/最小值做比较，我们把每两个元素要做的比较次数加少到了3次。

## 性能

这个算法的复杂度是  **O(n)**。数组里的每一个对象都需要进行比较，所以花费的时间与数组的长度成正比。

*原文出自 [Chris Pilcher](https://github.com/chris-pilcher)*