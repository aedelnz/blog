---
title: Markdown 示例
published: 2023-10-01
description: Markdown 博客文章的简单示例。
tags: [Markdown, 示例]
category: 示例
draft: false
---

# h1 标题

段落之间用空行分隔。

第二段。 _斜体_，**粗体**，和`等宽字体`。无序列表看起来像这样：

- 这个
- 那个
- 另一个

注意 --- 不考虑星号 --- 实际文本内容从第 4 列开始。

> 块引用
> 是这样写的。
>
> 它们可以跨越多个段落，
> 如果你愿意的话。

用 3 个破折号表示破折号。用 2 个破折号表示范围（例如，"都在第 12--14 章中"）。三个点 ... 会被转换为省略号。支持 Unicode。 ☺

## h2 标题

这是一个有序列表：

1. 第一项
2. 第二项
3. 第三项

再次注意实际文本如何从第 4 列开始（从左侧开始 4 个字符）。这是一个代码示例：

    # 让我重申一下 ...
    for i in 1 .. 10 { do-something(i) }

正如你可能猜到的，缩进 4 个空格。顺便说一下，如果你愿意，你可以使用定界块代替缩进块：

```
define foobar() {
    print "Welcome to flavor country!";
}
```

（这样更容易复制粘贴）。你可以选择标记定界块以便 Pandoc 对其进行语法高亮：

```python
import time
# 快，数到十！
for i in range(10):
    # （但不要 *太* 快）
    time.sleep(0.5)
    print i
```

### h3 标题

现在是一个嵌套列表：

1. 首先，准备这些食材：

    - 胡萝卜
    - 芹菜
    - 扁豆

2. 烧一些水。

3. 把所有东西倒进锅里，然后遵循
    这个算法：

        find wooden spoon
        uncover pot
        stir
        cover pot
        balance wooden spoon precariously on pot handle
        wait 10 minutes
        goto first step (or shut off burner when done)

    不要碰到木勺，否则它会掉下来。

再次注意文本总是如何按 4 空格缩进对齐的（包括上面继续第 3 项的最后一行）。

这里有一个链接，指向 [一个网站](http://foo.bar)，一个 [本地文档](local-doc.html)，以及 [当前文档中的一个章节标题](#h2-标题)。这里有一个脚注 [^1]。

[^1]: 脚注文本在这里。

表格可以这样：

size material color

---

9 leather brown
10 hemp canvas natural
11 glass transparent

表：鞋子，它们的尺寸，以及它们是由什么制成的

（上面是表格的标题）。Pandoc 还支持多行表格：

---

keyword text

---

red Sunsets, apples, and
other red or reddish
things.

green Leaves, grass, frogs
and other things it's
not easy being.

---

下面是一条水平线。

---

这里是一个定义列表：

apples
: 适合做苹果酱。
oranges
: 柑橘类！
tomatoes
: 西红柿里没有"e"。

同样，文本缩进 4 个空格。（在每个术语/定义对之间放一个空行，以便更分散）。

这里是一个"行块"：

| Line one
| Line too
| Line tree

图片可以这样指定：

[//]: # (![example image]&#40;./demo-banner.png "An exemplary image"&#41;)

行内数学公式这样放：$\omega = d\phi / dt$。显示数学应该单独占一行，放在双美元符号中：

$$I = \int \rho R^{2} dV$$

$$
\begin{equation*}
\pi
=3.1415926535
 \;8979323846\;2643383279\;5028841971\;6939937510\;5820974944
 \;5923078164\;0628620899\;8628034825\;3421170679\;\ldots
\end{equation*}
$$

注意，你可以用反斜杠转义任何你希望字面显示的标点字符，例如：\`foo\`、\*bar\* 等。
