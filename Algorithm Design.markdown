# 算法设计技巧

当你需要找准一种算法来解决一个全新的问题是应该怎么办？

### 它跟其他问题有相似之处吗？

If you can frame your problem in terms of another, more general problem, then you might be able to use an existing algorithm. Why reinvent the wheel?
如果你能把你的问题跟其他更普遍的问题归为一类，那你也许就能用一个已知的算法来处理它。为什么要重复造轮子呢？

One thing I like about [The Algorithm Design Manual](http://www.algorist.com) by Steven Skiena is that it includes a catalog of problems and solutions you can try. (See also his [algorithm repository](http://www3.cs.stonybrook.edu/~algorith/).)
其中一个让我喜欢 Steven Skiena 写的 [The Algorithm Design Manual](http://www.algorist.com) 的地方是，它提供了一系列你能用来练习的问题和对应的解法。（感兴趣的话可以看看他的[算法仓库](http://www3.cs.stonybrook.edu/~algorith/)。）

### 一开始用暴力解法是没问题的

Naive, brute force solutions are often too slow for practical use but they're a good starting point. By writing the brute force solution, you learn to understand what the problem is really all about.
单纯的、暴力的解法在实际场景中通常会比较慢，但它们是一个很好的出发点。写暴力解法能让你更好地了解到问题的全貌和根本。

Once you have a brute force implementation you can use that to verify that any improvements you come up with are correct. 
一旦你有了一种暴力解法，你就能以此为基础来验证你后续的一切改进是否真的有效。

And if you only work with small datasets, then a brute force approach may actually be good enough on its own. Don't fall into the trap of premature optimization!
况且，如果你只需要处理一个比较小的数据集，暴力解法说不定就已经足够了。千万别掉进了提前优化的陷阱里！

### 分而治之

>“当你改变了看待事物的角度，这个事物本身就改变了。”</br>
>马克斯·普朗克， 量子力学创始人之一、诺贝尔奖获得者

分治是一种自下而上解决问题的方式，它能帮助你把一个庞大的问题拆分成一个个容易处理的小碎片。

你不再把问题看成是一个巨大而复杂的单一整体，而是把它拆分成互相关联但却更容易理解和解决的小问题。

你只需要一点点解决掉这些小问题，它们积累起来就会帮助你看见这个大问题的正解。在得到最终的答案之前，你的每一步都是在卜丝抽茧，让解法更加清晰明了。

先掌握较小问题的解法，然后重复或递归地应用这个解法，能让你用更短的时间解决问题。
