# 关于大 O 标记法的笔记

了解一个算法有多快或需要占用多少空间是很有用的。这能帮助你选择到合适的算法。

大 O 标记法能让你对一个算法的运行时间和所需空间有一个粗略的了解。当别人说“这个算法最坏情况下的时间复杂度是 **O(n^2)** 并且会占用 **O(n)** 的空间”，他们的意思是这个算法比较慢但消耗的内存不多。

算法复杂度通常是经过数学分析得出来的。我们会跳过这里面的数学部分，但了解不同数值表示什么意思是很有用的，所以这里有一张简单的对照表。**n** 表示你要处理的数据元素的个数。比如说，当你要对一个含有100个元素的数组进行排序时，**n = 100**。

算法复杂度 | 别名 | 描述
------| ---- | -----------
**O(1)** | 常量级 | **这是最优的。**这个算法总是用相同的时间完成，不管数据量有多少。例子：通过下标在数组中查找一个元素。
**O(log n)** | 对数级 | **非常好。**这类算法每次迭代需要处理的数据量都会减半。如果你有100个数据元素，只需要7步就能找到正确答案。如果有1000个元素，则需要10步。如果是10000个元素，也只要20步。这是就算在数据量很大的情况下也是非常非常快的。例子：二分查找。
**O(n)** | 线性级 | **性能不错。**假设有100个数据元素，这个算法就需要100个单位的时间去计算。当数据量翻倍，它所需要的计算时间也会正好翻倍（200个单位的时间）。例子：顺序查找。
**O(n log n)** | "线性代数级" | **性能还行。**比线性级差一点，但是也不会太多。例子：常用的排序算法里最快的那些。
**O(n^2)** | 平方级 | **稍微有点慢了。**假设有100个数据元素，这个算法需要 100^2 = 10,000 个单位的时间去计算。当数据量翻倍，它所需要的计算时间会翻4倍（因为 2^2 = 4）。例子：用了多重循环的算法，比如插入排序。
**O(n^3)** | 次方级 | **性能较差。**假设有100个数据元素，这个算法就需要 100^3 = 1,000,000 个单位的时间去计算。当数据量翻倍，它所需要的计算时间会翻8倍。例子：矩阵乘法。
**O(2^n)** | 指数级 | **性能很差。**除非不可避免，你一般不会想用这些算法。增加哪怕一个数据元素都会让花费时间翻倍。例子：旅行商问题。
**O(n!)** | 阶乘级 | **慢得无法容忍。**它做什么都慢出天际。



![Comparison of Big O computations](https://upload.wikimedia.org/wikipedia/commons/7/7e/Comparison_computational_complexity.svg)



下面是每种算法复杂度的比较形象的例子：

**O(1)**

  最常见的 O(1) 复杂度的例子就是访问数组的下标。

  ```swift
  let value = array[5]
  ```

  还有一个 O(1) 的例子是压栈和出栈。


**O(log n)**

  ```swift
  var j = 1
  while j < n {
    // do constant time stuff
    j *= 2
  }
  ```  

  ‘J’ 不是简单的加1，而是在每个循环里都翻倍。

  二分查找算法就是 O(log n) 复杂度的常见例子。


**O(n)**

  ```swift
  for i in stride(from: 0, to: n, by: 1) {
    print(array[i])
  }
  ```

  数组反转和线性查找就是 O(n) 复杂度的。


**O(n log n)**

  ```swift
  for i in stride(from: 0, to: n, by: 1) {
  var j = 1
    while j < n {
      j *= 2
      // do constant time stuff
    }
  }
  ```

  OR

  ```swift
  for i in stride(from: 0, to: n, by: 1) {
    func index(after i: Int) -> Int? { // multiplies `i` by 2 until `i` >= `n`
      return i < n ? i * 2 : nil
    }
    for j in sequence(first: 1, next: index(after:)) {
      // do constant time stuff
    }
  }
  ```

  归并排序和堆排序就是 O(n log n) 复杂度的。


**O(n^2)**

  ```swift
  for i  in stride(from: 0, to: n, by: 1) {
    for j in stride(from: 1, to: n, by: 1) {
      // do constant time stuff
    }
  }
  ```

  Traversing a simple 2-D array and Bubble Sort are examples of O(n^2) complexity.
  翻转二维数组和冒泡排序就是 O(n^2) 复杂度的.


**O(n^3)**

  ```swift
  for i in stride(from: 0, to: n, by: 1) {
    for j in stride(from: 1, to: n, by: 1) {
      for k in stride(from: 1, to: n, by: 1) {
        // do constant time stuff
      }
    }
  }
  ```  

**O(2^n)**

  复杂是 O(2^n) 的算法通常都是用递归去求解的，换句话说，要解决 N 个数据量的问题，需要先递归解决 N-1 个数据量时的问题。
  下面的例子展示了一个有 N 个碟片的汉诺塔问题的解法。

  ```swift
  func solveHanoi(n: Int, from: String, to: String, spare: String) {
    guard n >= 1 else { return }
    if n > 1 {
        solveHanoi(n: n - 1, from: from, to: spare, spare: to)
        solveHanoi(n: n - 1, from: spare, to: to, spare: from)
    }
  }
  ```


**O(n!)**

  下面是一个 O(n!) 复杂度方法的例子。

  ```swift
  func nFactFunc(n: Int) {
    for i in stride(from: 0, to: n, by: 1) {
      nFactFunc(n: n - 1)
    }
  }
  ```

一般来说，在做很多数学计算之前，你就可以凭直觉感受出一个算法的复杂度。如果你的代码用到了一个单层循环去处理 **n** 个元素，那么它的算法复杂度就是 **O(n)**。如果代码里有一层循环嵌套在另一层里面，那么它的算法复杂度就是 **O(n^2)**。三层嵌套的循环的算法复杂度是 **O(n^3)**，以此类推。

值得注意的是，大 O 标记法是一种估算方式，它只适用于数据量很大的情况。比如说，[插入排序](Insertion%20Sort/)在最坏情况下的算法复杂度是 **O(n^2)**。
理论上，这比[归并排序](Merge%20Sort/) **O(n log n)** 的复杂度更高。然而对于较小的数据量来说，插入排序是更快的，当数组部分有序的时候更是如此。

如果你觉得不太理解，那也别让大 O 标记法阻碍了你的脚步。它在对比两个算法优劣的时候还算有用。但最终你还是要在实际情况下去测试它们的性能。而且，如果数据量相对较小，那就算是一个慢一点的算法也相当够用了。