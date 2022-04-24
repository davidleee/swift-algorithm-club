# 栈

> 这个主题的相关教程在[这里](https://www.raywenderlich.com/149213/swift-algorithm-club-swift-stack-data-structure)。

栈是一种功能受限的数组。你只能把新的元素 *push* 到栈的顶部（也被叫做*入栈*或*压栈*），或者把栈最顶部的元素 *pop* 出来（也被叫做*出栈*），还可以在不出栈的前提下用 *peek* 偷看一下栈最顶部的元素。

你为什么会需要这样一个东西呢？因为，在许多算法里，你会需要临时存放某些元素，未来合适的时候再取出来。而且大多数情况下，存取这些元素的顺序至关重要。

栈为你提供了 LIFO 或者叫做“后进先出”的元素访问机制。你最后放进去的元素，会在你下一次取的时候最先出来。（有种很相似的数据结构叫[队列](../Queue/)，它的元素访问机制是 FIFO 或者叫“先进先出”的。）

打个比方，我们放一个数字到栈里：

```swift
stack.push(10)
```

这个栈现在是这样的： `[ 10 ]`。放下一个数字：

```swift
stack.push(3)
```

这个栈现在是这样的： `[ 10, 3 ]`。再放一个数字：

```swift
stack.push(57)
```

这个栈现在是： `[ 10, 3, 57 ]`。现在来让最顶部的数字出栈：

```swift
stack.pop()
```

这个操作返回的是 `57`，因为这是我们最后入栈的数字。现在栈重新变成了 `[ 10, 3 ]`。

```swift
stack.pop()
```

这个操作返回的是 `3`，以此类推。如果栈是空的，出栈操作会返回 `nil`，或者在某些实现中它会抛出一个错误信息：“栈下溢”（"stack underflow"）。

在 Swift 里很容易实现一个栈。它只是一个对数组的封装，限制了你只能入栈、出栈和查看最顶部的元素。

```swift
public struct Stack<T> {
  fileprivate var array = [T]()

  public var isEmpty: Bool {
    return array.isEmpty
  }

  public var count: Int {
    return array.count
  }

  public mutating func push(_ element: T) {
    array.append(element)
  }

  public mutating func pop() -> T? {
    return array.popLast()
  }

  public var top: T? {
    return array.last
  }
}
```

需要注意的是，入栈是把元素加入到数组的末尾而不是开头。将元素插入到数组开头是很低效的，这是个 **O(n)** 复杂度的操作，因为它需要在内存中对数组里所有已有的元素进行移动。将元素添加到数组末尾的复杂度是 **O(1)**；不管数组有多长，这个操作永远都花费相同的时间。

关于栈的有趣小知识：每当你执行函数或调用方法时，CPU 都会把返回地址放到栈上。当函数结束，CPU 会用那个地址来跳回到它被调用的位置。这就是为什么如果你调用了太多的函数，比如说执行一个永不返回的递归操作，你就会得到所谓的“栈溢出”（"stack overflow"）错误，它是要告诉 CPU 使用的栈已经没有更多空间可用了。

*原文出自 Matthijs Hollemans*
