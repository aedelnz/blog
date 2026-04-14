---
title: 'Markdown 风格指南'
description: '这里是一些基本 Markdown 语法的示例，可以在使用 Astro 编写 Markdown 内容时使用。'
pubDate: 'Jun 19 2024'
heroImage: '../../assets/blog-placeholder-1.jpg'
---

这里是一些基本 Markdown 语法的示例，可以在使用 Astro 编写 Markdown 内容时使用。

## 标题

以下 HTML `<h1>`—`<h6>` 元素表示六个级别的章节标题。`<h1>` 是最高的章节级别，而 `<h6>` 是最低的。

# H1

## H2

### H3

#### H4

##### H5

###### H6

## 段落

君子曰：学不可以已。青，取之于蓝，而青于蓝；冰，水为之，而寒于水。木直中绳，輮以为轮，其曲中规。虽有槁暴，不复挺者，輮使之然也。故木受绳则直，金就砺则利，君子博学而日参省乎己，则知明而行无过矣。

出自《荀子・劝学》。

## 图片

### 语法

```markdown
![替代文本](./full/or/relative/path/of/image)
```

### 输出

![图片](../../assets/blog-placeholder-1.jpg)

## 引用

引用元素表示从另一个来源引用的内容，可选引用来源（必须在 `footer` 或 `cite` 元素内），可选内内变化（如注释和缩写）。

### 引用无作者

#### 语法

```markdown
> Tiam, ad mint andaepu dandae nostion secatur sequo quae.  
> **Note** that you can use _Markdown syntax_ within a blockquote.
```

#### 输出

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.  
> **Note** that you can use _Markdown syntax_ within a blockquote.

### 引用有作者

#### 语法

```markdown
> Don't communicate by sharing memory, share memory by communicating.<br>
> — <cite>Rob Pike[^1]</cite>
```

#### 输出

> Don't communicate by sharing memory, share memory by communicating.<br>
> — <cite>Rob Pike[^1]</cite>

[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## 表格

### 语法

```markdown
| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |
```

### 输出

| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |

## 代码块

### 语法

we can use 3 backticks ``` in new line and write snippet and close with 3 backticks on new line and to highlight language specific syntax, write one word of language name after first 3 backticks, for eg. html, javascript, css, markdown, typescript, txt, bash

````markdown
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Example HTML5 Document</title>
  </head>
  <body>
    <p>Test</p>
  </body>
</html>
```
````

### 输出

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Example HTML5 Document</title>
  </head>
  <body>
    <p>Test</p>
  </body>
</html>
```

## 列表类型

### 有序列表

#### 语法

```markdown
1. First item
2. Second item
3. Third item
```

#### 输出

1. First item
2. Second item
3. Third item

### 序列表

#### 语法

```markdown
- List item
- Another item
- And another item
```

#### 输出

- List item
- Another item
- And another item

### 嵌套列表

#### 语法

```markdown
- Fruit
  - Apple
  - Orange
  - Banana
- Dairy
  - Milk
  - Cheese
```

#### 输出

- Fruit
  - Apple
  - Orange
  - Banana
- Dairy
  - Milk
  - Cheese

## 其他元素 — abbr, sub, sup, kbd, mark

### 语法

```markdown
<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>Delete</kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.
```

### 输出

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>Delete</kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.
