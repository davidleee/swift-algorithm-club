# 摩尔字符串搜索

> 这个主题的相关教程在[这里](https://www.raywenderlich.com/163964/swift-algorithm-club-booyer-moore-string-search-algorithm)

目标：在不引入 Foundation 库和不使用 `NSString` 的 `rangeOfString()` 方法的前提下，只用 Swift 实现一个字符串搜索算法。

换句话说，我们想要给 `String` 实现一个扩展方法 `indexOf(pattern: String)` 并返回第一个搜索到的目标模式的 `String.Index`，或者在找不到的情况下返回 `nil`。

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

[穷举法](../Brute-Force%20String%20Search/)已经能用了，但它并不是那么高效，特别是要搜索大量字符串的时候。其实你不需要检索源字符串里的 _每一个_ 字符 —— 你总是能找到机会跳过一些字符。

这个能跳过字符的算法就是已经存在了很久的[摩尔字符串搜索](https://en.wikipedia.org/wiki/Boyer–Moore_string_search_algorithm)。它还被用作所有字符串搜索算法的性能基准。

下面是其中一种用 Swift 实现的方式：

> **译者注：** 为了避免混淆，提前做个名词解释，假设我们要在一篇文章中找某个名字：“目标字符串”指的是我们要找的内容，相当于我们在搜索框里输入的名字；“源字符串”指的是我们要发起查询的地方，相当于这整篇文章。

```swift
extension String {
    func index(of pattern: String) -> Index? {
        // 把目标字符串长度保存起来，因为我们接下来会多次用上它，而计算长度是比较昂贵的操作
        let patternLength = pattern.count
        guard patternLength > 0, patternLength <= count else { return nil }

        // 构造“跳过查询表”。
        // 这个表用于决定当目标字符串里的某个字符没找到时，我们应该跳过多少个字符
        var skipTable = [Character: Int]()
        for (i, c) in pattern.enumerated() {
            skipTable[c] = patternLength - i - 1
        }

        // 这指向了目标字符串里的最后一个字符
        let p = pattern.index(before: pattern.endIndex)
        let lastChar = pattern[p]

        // 因为我们会从右往左扫描目标字符串，所以我们会先往前跳过目标字符串长度那么长的距离。
        // 减 1 是因为 startIndex 已经指向了源字符串里的第一个字符
        var i = index(startIndex, offsetBy: patternLength - 1)

        // 这个工具方法用来同时反向遍历两个字符串，
        // 直到找到它们字符不相同的位置，或者直到下标去到目标字符串的开头
        func backwards() -> Index? {
            var q = p
            var j = i
            while q > pattern.startIndex {
                j = index(before: j)
                q = index(before: q)
                if self[j] != pattern[q] { return nil }
            }
            return j
        }

        // 主循环。会一直检索到源字符串的结尾。
        while i < endIndex {
            let c = self[i]

            // 现在这个字符跟目标字符串里的最后一个字符一样吗？
            if c == lastChar {

                // 有可能会匹配。直接简单粗暴地从后往前对比。
                if let k = backwards() { return k }

                // 如果不匹配，那我们只能往前跳过一个字符。
                i = index(after: i)
            } else {
                // 字符不一致，所以下标往前走。具体要跳过多少个字符由之前构造的查询表决定。
                // 如果这个字符没出现在目标字符串里，那我们可以跳过整个目标字符串那么长。
                // 但是，如果这个字符有出现在目标字符串里，说明接下来还有可能会被匹配上，所以我们就不能跳过那么多字符了。
                i = index(i, offsetBy: skipTable[c] ?? patternLength, limitedBy: endIndex) ?? endIndex
            }
        }
        return nil
    }
}
```

算法是这样工作的。把目标字符串和源字符串排在一起，然后看看哪个字符跟目标字符串里 _最后_ 那个字符一样：

```
source string:  Hello, World
search pattern: World
                    ^
```

> **译者注：** 为了对齐文字和下面的标记，这里的源字符串（source string）和目标字符串（search pattern）就没有翻译了。

这会有三种可能的情况：

1. 两个字符是一样的，你发现了匹配的可能性。

2. 两个字符不一样，但是源字符串里的字符在目标字符串里的其他位置里出现过。

3. 源字符串里的字符没有出现在目标字符串里。

在这个例子里，字符 `o` 和 `d` 不一样，但是 `o` 存在于目标字符串里的其他位置。这说明我们可以往前跳过一些字符：

```
source string:  Hello, World
search pattern:    World
                       ^
```

注意看两个 `o` 现在是对齐的。再来，你需要比较目标字符串里的最后一个字符和正在检索的字符：`d` vs `W`。它们不一样，但是 `W` 有出现在目标字符串里。所以往前跳过字符，让两个 `W` 对齐：

```
source string:  Hello, World
search pattern:        World
                           ^
```

这次两个字符是一样的，而且有匹配的可能性。为了判断它们是不是真的匹配，你需要从后往前执行一个穷举法检索。然后这次搜索的工作就全部完成了。

在任何时候，往前跳过多少个字符都是由“跳过查询表”决定的，它是一个存有目标字符串里所有字符和跳过距离的字典。上述例子里的查询表是这样的：

```
W: 4
o: 3
r: 2
l: 1
d: 0
```

字符越靠近字符串的结尾，跳过的距离就越短。如果某个字符在字符串里出现了不止一次，那么跳过的距离就以接近字符串结尾的那一个为准。

> **注意：** 如果目标字符串只有很少几个字符，直接用简单的穷举法会更快。在处理很短的目标字符串时，需要权衡构建跳过查询表和直接穷举法之间的优劣。

> 这段代码基于《Dr Dobb's》杂志于1989年7月发表的文章 ["Faster String Searches" by Costas Menico](http://www.drdobbs.com/database/faster-string-searches/184408171) ——你没看错，1989年！有时候留着那些旧杂志还是挺有用的。

相关信息：算法的[一份详细分析报告](http://www.inf.fh-flensburg.de/lang/algorithmen/pattern/bmen.htm)。

## Horspool 算法

上述算法的一种变体叫 [Horspool 算法](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore%E2%80%93Horspool_algorithm)。


就像原始的摩尔算法那样，它也用了 `skipTable` 来跳过一些字符串。区别在于我们怎么判断部分匹配。在上面的版本里，如果找到了部分而不是完整的匹配，我们就只往前跳一个字符。在这个修改版本里，我们在这时候也用跳过查询表区判断。

下面是 Horspool 算法的实现：

```swift
extension String {
    func index(of pattern: String) -> Index? {
        // 把目标字符串长度保存起来，因为我们接下来会多次用上它，而计算长度是比较昂贵的操作
        let patternLength = pattern.count
        guard patternLength > 0, patternLength <= characters.count else { return nil }

        // 构造“跳过查询表”。
        // 这个表用于决定当目标字符串里的某个字符没找到时，我们应该跳过多少个字符
        var skipTable = [Character: Int]()
        for (i, c) in pattern.enumerated() {
            skipTable[c] = patternLength - i - 1
        }

        // 这指向了目标字符串里的最后一个字符
        let p = pattern.index(before: pattern.endIndex)
        let lastChar = pattern[p]

        // 因为我们会从右往左扫描目标字符串，所以我们会先往前跳过目标字符串长度那么长的距离。
        // 减 1 是因为 startIndex 已经指向了源字符串里的第一个字符
        var i = index(startIndex, offsetBy: patternLength - 1)

        // 这个工具方法用来同时反向遍历两个字符串，
        // 直到找到它们字符不相同的位置，或者直到下标去到目标字符串的开头
        func backwards() -> Index? {
            var q = p
            var j = i
            while q > pattern.startIndex {
                j = index(before: j)
                q = index(before: q)
                if self[j] != pattern[q] { return nil }
            }
            return j
        }

        // 主循环。会一直检索到源字符串的结尾。
        while i < endIndex {
            let c = self[i]

            // 现在这个字符跟目标字符串里的最后一个字符一样吗？
            if c == lastChar {

                // 有可能会匹配。直接简单粗暴地从后往前对比。
                if let k = backwards() { return k }

                // 确保至少会跳过一个字符
                // 这是很有必要的，因为第一个字符也在查询表里，而且 `skipTable[lastChar] = 0`
                let jumpOffset = max(skipTable[c] ?? patternLength, 1)
                i = index(i, offsetBy: jumpOffset, limitedBy: endIndex) ?? endIndex
            } else {
                // 字符不一致，所以下标往前走。具体要跳过多少个字符由之前构造的查询表决定。
                // 如果这个字符没出现在目标字符串里，那我们可以跳过整个目标字符串那么长。
                // 但是，如果这个字符有出现在目标字符串里，说明接下来还有可能会被匹配上，所以我们就不能跳过那么多字符了。
                i = index(i, offsetBy: skipTable[c] ?? patternLength, limitedBy: endIndex) ?? endIndex
            }
        }
        return nil
    }
}
```

在实际使用时，摩尔算法的 Horspool 版本比原版的性能会好上那么一点点。然而，这还是取决于你的权衡取舍。

> 这段代码基于论文：[R. N. Horspool (1980). "Practical fast searching in strings". Software - Practice & Experience 10 (6): 501–506.](http://www.cin.br/~paguso/courses/if767/bib/Horspool_1980.pdf)

*原文出自 Kelvin Lau。由 Matthijs Hollemans 进行补充，并由 Andreas Neusüß、[Matías Mazzei](https://github.com/mmazzei) 更新*