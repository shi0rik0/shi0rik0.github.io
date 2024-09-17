---
title: "现代HTML的最简模板"
date: 2024-09-17T10:58:17+08:00
draft: false
---

下面是使用Vite创建的前端项目的默认`index.html`文件内容：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <link rel="icon" href="/favicon.ico">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vite App</title>
</head>

<body>
</body>

</html>
```

下面对这里面的一些关键点进行解说。

```html
<!DOCTYPE html>
```

这一行表示该文件采用的是HTML 5标准。

```html
<html lang="en">
```

`lang="en"`表示这个网页是英文网页，如果是大陆的中文网页，可以写`lang="zh-CN"`。

```html
<meta charset="UTF-8">
```

这一行表示源文件使用的编码是UTF-8。

```html
<link rel="icon" href="/favicon.ico">
```

这一行指定了网页的图标。

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

这一行是对网页的viewport进行设置，使得网页在移动设备上浏览时不会缩放。由于早期大部分网页没有针对移动设备的小屏幕进行适配，因此移动设备访问网页时，默认会渲染一个大于屏幕尺寸的网页，然后用户可以通过手指触摸操作来移动网页的显示区域。由于现在很多网页都是原生适配移动端的，在这种情况下缩放就没有必要了，可以通过这个设置来禁用缩放。


```html
<title>Vite App</title>
```

这一行指定了网页的标题。
