---
title: "在flex布局中实现按比例分配宽度或高度"
date: 2025-01-08T22:05:13+08:00
draft: false
---

## 需求

假设我们有这样的布局：

```html
<div class="flex flex-col w-[400px] h-[400px]">
  <div>div 1</div>
  <div>div 2</div>
  <div>div 3</div>
</div>
```

我们希望实现这样的效果：三个 div 的高度固定按照 1:2:3 的比例分配。

## 解决方案

```html
<div class="flex flex-col w-[400px] h-[400px]">
  <div class="flex-1 min-h-0 overflow-clip">div 1</div>
  <div class="flex-[2_0_0] min-h-0 overflow-clip">div 2</div>
  <div class="flex-[3_0_0] min-h-0 overflow-clip">div 3</div>
</div>
```

## 要点解析

### flex-grow 和 flex-basis

在 Tailwind CSS 中，class `flex-1` 表示 CSS 中的属性 `flex: 1 1 0%`，`flex-[2_0_0]` 表示 `flex: 2 0 0`，`flex-[3_0_0]` 表示 `flex: 3 0 0`。而 CSS 中`flex: X Y Z`是一个缩写，相当于：

```css
flex-grow: X;
flex-shrink: Y;
flex-basis: Z;
```

其中，`flex-grow` 表示元素的放大比例，`flex-shrink` 表示元素的缩小比例，`flex-basis` 表示元素的基础大小。

下面举个例子来说明`flex-grow`和`flex-basis`的作用，`flex-shrink`的道理是类似的。

考虑下面的布局：

```html
<div class="flex flex-col h-[400px]">
  <div class="flex-[2_0_100px]">div 1</div>
  <div class="flex-[1_0_0]">div 2</div>
</div>
```

父元素的高度是 400px，而 div 1 的 basis 是 100px，div 2 的 basis 是 0。因此，减去基础高度之后，父元素还剩下 300px 的高度可以分配，这些空间按照 2:1 的比例分配给 div 1 和 div 2，所以 div 1 的分配到的高度是 200px，div 2 是 100px。最终的效果是 div 1 的高度是 100 + 200px = 300px，div 2 的高度是 0 + 100px = 100px。

### min-h-0

在解决方案中，我们还设置了`min-h-0`，也就是`min-height: 0`。这是因为在 flex 布局中，如果子元素的内容超出了子元素原本的高度，子元素会被强制拉伸，从而破坏比例分配。因此，我们需要设置`min-height: 0`来让溢出的内容不会影响到父元素的高度。

### overflow-clip

经过上面的设置，现在 div block 的高度已经没问题了，但是 div 里面的内容还是可能会溢出。因为在 HTML 中，并没有规定元素的内容（比如文本）不能超出元素的边界。因此，我们需要设置`overflow-clip`来截断溢出的内容，也可以设置`overflow-auto`来显示一个滚动条。

### 关于 padding 和 border 要注意的地方

这里有一个细节，就是 padding 和 border 也会影响元素的基础宽度或高度，从而破坏比例分配。StackOverflow 上有一个专门讨论这个问题的帖子：[Why is padding expanding a flex item?](https://stackoverflow.com/questions/33145242/why-is-padding-expanding-a-flex-item)。

简单来说，问题出在`flex-basis`的计算上。最终生效的`flex-basis`的值是按照下面的公式计算的：

```
real_flex_basis = max(flex_basis, padding + border)
```

下面是一些解决办法：

1. 如果没有强迫症的话，就不管这个问题。反正一般情况下 padding 和 border 不会太大，不会对比例分配产生太大的影响。
2. 设置 padding 和 border 都为 0。
3. 如果需要设置 padding 或 border，可以手动将`flex-basis`设置成一个符合比例的同时又略微大于 padding + border 的值。
