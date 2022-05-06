# 队列

> 这个主题的相关教程在[这里](https://www.raywenderlich.com/148141/swift-algorithm-club-swift-queue-data-structure)。

队列是一个只能在末尾添加新元素和在开头取出旧元素的列表。这确保了你第一个取出的元素正是你第一个存入的元素。这就是所谓的“先来后到”。

你为什么需要这样一个东西呢？因为，在许多算法里，你会需要临时存放某些元素，未来合适的时候再取出来。而且大多数情况下，存取这些元素的顺序至关重要。

队列为你提供了 FIFO 或者叫做“先进先出”的元素访问机制。你最早放进去的元素，会在你下一次取的时候最先出来。这才公平嘛！（有种很相似的数据结构叫[栈](../Stack/)，它的元素访问机制是 LIFO 或者叫“后进先出”的。）

下面的例子里展示了如何将一个数字加入队列：

```swift
queue.enqueue(10)
```

这个队列现在是这样的： `[ 10 ]`。放下一个数字：

```swift
queue.enqueue(3)
```

这个队列现在是这样的： `[ 10, 3 ]`。再放一个数字：

```swift
queue.enqueue(57)
```

这个队列现在是： `[ 10, 3, 57 ]`。现在来取出队列开头的第一个元素：

```swift
queue.dequeue()
```

这个操作返回的是 `10`，因为这是我们最先加入的数字。现在队列重新变成了 `[ 3, 57 ]`。所有元素都往前移动了一个位置。

```swift
queue.dequeue()
```

这个操作返回的是 `3`，下一次出队列操作会返回 `57`，以此类推。如果队列是空的，出栈操作会返回 `nil`，或者在某些实现中它会抛出一个错误信息。

> **注意：** 队列并不总是最佳选择。如果数据添加和取出的顺序不重要的话，你可以用[栈](../Stack/)来代替队列。栈在使用上会更简单和快速。

## 代码

这是用 Swift 实现的一个简易的队列。它是一个对数组的封装，它实现了入队列、出队列和查看最顶部元素的方法：

```swift
public struct Queue<T> {
  fileprivate var array = [T]()

  public var isEmpty: Bool {
    return array.isEmpty
  }
  
  public var count: Int {
    return array.count
  }

  public mutating func enqueue(_ element: T) {
    array.append(element)
  }
  
  public mutating func dequeue() -> T? {
    if isEmpty {
      return nil
    } else {
      return array.removeFirst()
    }
  }
  
  public var front: T? {
    return array.first
  }
}
```

这个队列能用，但性能上还有提升空间。

入队列是一个 **O(1)** 的操作，因为往数组的末尾添加元素所需要耗费的时间是恒定的，不管这个数组有多长。

你可能会好奇为什么把元素添加到数组末尾是复杂度为 **O(1)** 的操作或者说它的耗时是恒定的。这是因为 Swift 里的数组结尾永远都有额外的空闲空间，比方说我们这样做：

```swift
var queue = Queue<String>()
queue.enqueue("Ada")
queue.enqueue("Steve")
queue.enqueue("Tim")
```

那么这个数组真实的样子可能是这样的：

	[ "Ada", "Steve", "Tim", xxx, xxx, xxx ]

其中 `xxx` 表示预留出来的闲置的内存。添加新元素到数组里的时候，下一个闲置的空间就会被填上：

	[ "Ada", "Steve", "Tim", "Grace", xxx, xxx ]

这是内存拷贝的结果，所以是一个耗时恒定的操作。

每次数组预留的出来的空间都是有限的。当最后一个 `xxx` 被填上，而你还想继续添加新元素的时候，数组就需要调整自身的大小以获取更多的空间。

调整数组的大小需要做两个动作：分配新内存和将原有数据拷贝到新位置。这是一个相对比较慢的 **O(n)** 的过程。因为这种情况只会偶尔出现，所以添加新元素到数组末尾的复杂度平均下来还是 **O(1)** 的。

出队列就是另一个故事了。出队列意味着我们要把元素从数组的开头取出来。这永远都是一个 **O(n)** 复杂度的操作，因为它需要移动其余元素在内存中位置。

在我们的例子里，取出第一个元素 `"Ada"` 后，`"Steve"` 就会被拷贝到 `"Ada"` 原来的位置上，`"Tim"` 会被拷贝到 `"Steve"` 原来的位置上，然后 `"Grace"` 会被拷贝到 `"Tim"` 原来的位置上：

	before   [ "Ada", "Steve", "Tim", "Grace", xxx, xxx ]
	                   /       /      /
	                  /       /      /
	                 /       /      /
	                /       /      /
	 after   [ "Steve", "Tim", "Grace", xxx, xxx, xxx ]
 
在内存中移动所有这些元素是一个 **O(n)** 复杂度的操作。所以在我们的简易队列里，入队列是高效的，但出队列的效率就有待加强了。

## 一种更高效的出队列方式

为了让出队列更高效，我们可以像上面提到的那样预留一些空间，不过这次是给数组的开头留。这个逻辑需要我们自己实现，因为 Swift 自己的数组没有这个功能。

这个方式的中心思想是：在有元素出队列之后，我们不再移动剩余的元素（速度慢），而是把这个出队列的元素的位置标志成空闲的（速度快）。于是当我们把 `"Ada"` 出栈之后，数组就变成了：

	[ xxx, "Steve", "Tim", "Grace", xxx, xxx ]

当我们再把 `"Steve"` 出栈之后，数组就变成了：

	[ xxx, xxx, "Tim", "Grace", xxx, xxx ]

因为数组开头的那些闲置的空间一直没有被回收利用，所以你可以时不时地用移动元素的方式裁剪一下数组：

	[ "Tim", "Grace", xxx, xxx, xxx, xxx ]

这个裁剪依然是一个 **O(n)** 复杂度的操作。但因为你只是偶尔给它来上这么一下，所以平均下来，出队列还是 **O(1)** 复杂度的。

下面是这个新版本的队列实现：

```swift
public struct Queue<T> {
  fileprivate var array = [T?]()
  fileprivate var head = 0
  
  public var isEmpty: Bool {
    return count == 0
  }

  public var count: Int {
    return array.count - head
  }
  
  public mutating func enqueue(_ element: T) {
    array.append(element)
  }
  
  public mutating func dequeue() -> T? {
    guard head < array.count, let element = array[head] else { return nil }

    array[head] = nil
    head += 1

    let percentage = Double(head)/Double(array.count)
    if array.count > 50 && percentage > 0.25 {
      array.removeFirst(head)
      head = 0
    }
    
    return element
  }
  
  public var front: T? {
    if isEmpty {
      return nil
    } else {
      return array[head]
    }
  }
}
```

现在这个数组保存的元素类型从 `T` 变成了 `T?`，这是因为我们要把数组的某些下标置空。`head` 这个变量保存着数组中第一个有值元素的下标。

大部分新逻辑都在 `dequeue()` 里面。当我们执行出队列操作时，我们首先把 `array[head]` 设置为 `nil` 从而把这个元素从数组里移除。然后我们把 `head` 增加1，因为数组里的下一个元素现在变成队列中第一个元素了。

我们从这个状态：

	[ "Ada", "Steve", "Tim", "Grace", xxx, xxx ]
	  head

来到了这个状态：

	[ xxx, "Steve", "Tim", "Grace", xxx, xxx ]
	        head

这个场景就像是在超市收银台排队，从前是收银台不动，排队的人一个个往前走移动到收银台交钱；现在是队伍不动，收银台一个个人的往后移动去收钱。

如果我们不处理那些空闲的位置，那数组就会在反复入队列和出队列的过程中变得越来越长。为了偶尔裁剪一下数组，我们用到这个逻辑：

```swift
    let percentage = Double(head)/Double(array.count)
    if array.count > 50 && percentage > 0.25 {
      array.removeFirst(head)
      head = 0
    }
```

这段代码计算出了数组开头那些空位占数组总长度的比例。如果这个比例超过25%，那我们就把这些多余的空间砍掉。不过，如果数组很小的话，我们也没有必要去经常裁剪它，所以我们还做出了限制：只在数组长度超过50才会尝试去裁剪。

> **注意：** 这些数字我都是拍脑门定下来的。你可能需要针对你自己的应用和生产环境的情况进行调整。

如果想要在 playground 里测试一下这段代码，可以这样：

```swift
var q = Queue<String>()
q.array                   // [] empty array

q.enqueue("Ada")
q.enqueue("Steve")
q.enqueue("Tim")
q.array             // [{Some "Ada"}, {Some "Steve"}, {Some "Tim"}]
q.count             // 3

q.dequeue()         // "Ada"
q.array             // [nil, {Some "Steve"}, {Some "Tim"}]
q.count             // 2

q.dequeue()         // "Steve"
q.array             // [nil, nil, {Some "Tim"}]
q.count             // 1

q.enqueue("Grace")
q.array             // [nil, nil, {Some "Tim"}, {Some "Grace"}]
q.count             // 2
```

要测试裁剪的效果，把下面这行代码：

```swift
    if array.count > 50 && percentage > 0.25 {
```

替换成：

```swift
    if head > 2 {
```

现在当你再把一个对象出队列，数组就会变成这样：

```swift
q.dequeue()         // "Tim"
q.array             // [{Some "Grace"}]
q.count             // 1
```

数组开头的那些 `nil` 对象被移除了，空间再也不会被浪费了。这个新版的队列比原先那个也复杂不了多少，但现在它出队列的复杂度也是 **O(1)** 的了，这仅仅是因为我们稍微留心了一下数组的使用方式。

## 相关信息

创建队列的方式有很多中。其他的实现还可以是基于[链表](../Linked%20List/)、[环形缓冲区](../Ring%20Buffer/)或者[堆](../Heap/)的。

[双端队列](../Deque/)是这个主题的一种变体，你可以从它的两端进行出队列和入队列操作；还有[优先队列](../Priority%20Queue/)，一种有序的、最重要元素永远在队头的队列。

*原文出自 Matthijs Hollemans*
