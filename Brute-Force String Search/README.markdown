# 穷举法字符串搜索

如果你需要用纯粹的 Swift 来实现一个字符串搜索算法，而且不允许引入 Foundation 库和不允许使用 `NSString` 的 `rangeOfString()` 方法，可以怎么写呢？

我们的目标是给 `String` 实现一个扩展方法 `indexOf(pattern: String)` 并返回第一个搜索到的目标模式的 `String.Index`，或者在找不到的情况下返回 `nil`。
 
比方说：

```swift
// 输入：
let s = "Hello, World"
s.indexOf(pattern: "World")

// 输出：
<String.Index?> 7

// 输入：
let animals = "🐶🐔🐷🐮🐱"
animals.indexOf(pattern: "🐮")

// 输出：
<String.Index?> 6
```

> **注意：** 🐮 的下标是 6，而不是预料中的 3，是因为字符串在存储表情符号的时候会使用更多的空间。不过既然找到了正确的字符，`String.Index` 的具体数值就不重要了。

下面是一种穷举的解法：

```swift
extension String {
  func indexOf(_ pattern: String) -> String.Index? {
    for i in self.characters.indices {
        var j = i
        var found = true
        for p in pattern.characters.indices{
            if j == self.characters.endIndex || self[j] != pattern[p] {
                found = false
                break
            } else {
                j = self.characters.index(after: j)
            }
        }
        if found {
            return i
        }
    }
    return nil
  }
}
```

这会按顺序检查源字符串里的每一个字符。如果字符与目标字符串里的第一个字符相同，那么内循环就会检查其余字符是不是都相同。如果内循环没能完成任务，那么外循环就会从刚刚的位置继续执行。这个过程会不断重复直到找到了目标字符串或者已经到达了源字符串的结尾。

穷举法是能用的，但它并不是那么高效（或者说挺低效的）。不过，在处理比较短的字符串时还是可以用的。如果想要更高效处理更大量的文字，就去看看[摩尔字符串](../Boyer-Moore-Horspool)搜索吧。

*原文出自 Matthijs Hollemans*
