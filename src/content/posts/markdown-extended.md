---
title: Markdown 扩展功能
published: 2024-05-01
updated: 2024-11-29
description: 了解 Fuwari 中更多的 Markdown 功能
image: ''
tags: [示例, Markdown, Fuwari]
category: '示例'
draft: false
---

## GitHub 仓库卡片

你可以添加链接到 GitHub 仓库的动态卡片，页面加载时会从 GitHub API 拉取仓库信息。

::github{repo="saicaca/fuwari"}

使用代码 `::github{repo="<owner>/<repo>"}` 创建 GitHub 仓库卡片。

```markdown
::github{repo="saicaca/fuwari"}
```

## 提示框

支持以下类型的提示框：`note` `tip` `important` `warning` `caution`

:::note
突出显示用户即使在快速浏览时也应该考虑的信息。
:::

:::tip
帮助用户更成功的可选信息。
:::

:::important
用户成功所需的关键信息。
:::

:::warning
由于潜在风险需要用户立即关注的重要内容。
:::

:::caution
某个操作的潜在负面后果。
:::

### 基本语法

```markdown
:::note
突出显示用户即使在快速浏览时也应该考虑的信息。
:::

:::tip
帮助用户更成功的可选信息。
:::
```

### 自定义标题

提示框的标题可以自定义。

:::note[我的自定义标题]
这是一个带有自定义标题的提示框。
:::

```markdown
:::note[我的自定义标题]
这是一个带有自定义标题的提示框。
:::
```

### GitHub 语法

> [!TIP]
> 也支持 [GitHub 语法](https://github.com/orgs/community/discussions/16925)。

```
> [!NOTE]
> 也支持 GitHub 语法。

> [!TIP]
> 也支持 GitHub 语法。
```

### 剧透

你可以为文本添加剧透效果。文本也支持 **Markdown** 语法。

内容 :spoiler[被隐藏了 **哎呀**]！

```markdown
内容 :spoiler[被隐藏了 **哎呀**]！

```
