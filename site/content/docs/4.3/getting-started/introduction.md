---
layout: docs
title: 简介 | 译：@Turkyden
description: 通过 BootstrapCDN 以及入门模板页，使用 Bootstrap 这款全世界最流行的框架，快速搭建用于构建响应式，移动优先的网站。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 快速开始

想知道如何快速添加 Bootstrap 到你的项目中？使用 BootstrapCDN 所提供免费的全球静态资源加速服务。使用包管理器，或者需要下载源码？[请查看下载页面]({{< docsref "/getting-started/download" >}})。

### CSS

复制粘贴以下样式表 `<link>` 插入到 `<head>` 中，确保在所有其他样式表之前加载 CSS。

{{< highlight html >}}
<link rel="stylesheet" href="{{< param "cdn.css" >}}" integrity="{{< param "cdn.css_hash" >}}" crossorigin="anonymous">
{{< /highlight >}}

### JS

我们很多组件需要使用 JavaScript 去实现。特别是，他们依赖我们自己的 JavaScript 插件和 [Popper.js](https://popper.js.org/)。在你的页面底部引入 `<script>`，并确保在 `</body>` 标签内让它们执行。Popper.js 必须优先引入，然后才能引入别的 JavaScript 插件。

{{< highlight html >}}
<script src="{{< param "cdn.popper" >}}" integrity="{{< param "cdn.popper_hash" >}}" crossorigin="anonymous"></script>
<script src="{{< param "cdn.js" >}}" integrity="{{< param "cdn.js_hash" >}}" crossorigin="anonymous"></script>
{{< /highlight >}}

如果你使用 `<script type="module">`，请先查看[使用 Bootstrap 作为模块]({{< docsref "/getting-started/javascript#using-bootstrap-as-a-module" >}})章节。

想知道哪一个组件明确地依赖我们的 JavaScript 和 Popper.js ？点击以下展示组件链接。如果你对一般页面结构一直不清楚，请继续阅读示例页面模板。

我们的 `bootstrap.bundle.js` and `bootstrap.bundle.min.js` 包括了 [Popper](https://popper.js.org/)。有关 Bootstrap 中包含的内容，请参阅我们的 [contents]({{< docsref "/getting-started/contents#precompiled-bootstrap" >}}) 部分。

{{< partial "getting-started/components-requiring-javascript" >}}

## 入门模板

请确保你的你的页面使用最先进的设计和开发标准。那就意味着使用 HTML5 文档类型和包含一个能够适配响应式特性的视口 meta 标签。把他们放到一起，页面看起来应该是这样的：

{{< highlight html >}}
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="{{< param "cdn.css" >}}" integrity="{{< param "cdn.css_hash" >}}" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- Popper.js first, then Bootstrap JS -->
    <script src="{{< param "cdn.popper" >}}" integrity="{{< param "cdn.popper_hash" >}}" crossorigin="anonymous"></script>
    <script src="{{< param "cdn.js" >}}" integrity="{{< param "cdn.js_hash" >}}" crossorigin="anonymous"></script>
  </body>
</html>
{{< /highlight >}}

好了，如果你需要整个页面。请参阅[布局文档]({{< docsref "/layout/overview" >}})或者[我们的官方范例]({{< docsref "/examples" >}})去编排和铺设你的网站内容和组件。

## 重要的全局特性

Bootstrap 使用了一些重要的全局样式和设置，使用它们时需要注意，所有这些样式和设置几乎都专门针对跨浏览器样式的 *规范化* 。让我们继续深入。

### HTML5 doctype

Bootstrap 需要使用 HTML5 doctype。如果没有它，你会看到一些常见的不完整样式，而它不应该造成任何大的尴尬场面。

{{< highlight html >}}
<!doctype html>
<html lang="en">
  ...
</html>
{{< /highlight >}}

### 响应式 meta 标签

Bootstrap 是为 *移动优先* 而开发，这个策略是我们首先优化移动设备的代码，然后使用 CSS 媒体查询根据需要扩展组件，为确保所有设备的正确渲染和触摸缩放，需要将**响应式视口 meta 标签**添加到 `<head>`。

{{< highlight html >}}
<meta name="viewport" content="width=device-width, initial-scale=1">
{{< /highlight >}}

你可以查看关于这个的[入门模板](#starter-template)。

### Box-sizing

为了在 CSS 中更直接地进行大小调整，我们将全局的 `box-sizing` 值从 `content-box` 切换到 `border-box`。这样可以确保 `padding` 不会影响元素的最终计算宽度，但它可能会导致谷歌地图和谷歌自定义搜索引擎等第三方软件出现问题。

在极少数情况下您需要覆盖它，请使用以下内容：

{{< highlight css >}}
.selector-for-some-widget {
  box-sizing: content-box;
}
{{< /highlight >}}

使用以上片段，嵌套元素—包括生成的内容 `::before` 和 `::after`—将全部继承 `.selector-for-some-widget` 上的 `box-sizing` 的设定。

了解更多关于[盒子模型与 CSS 尺寸控制技巧](https://css-tricks.com/box-sizing/)。

### Reboot 重置

为了提升跨浏览器渲染能力，我们使用 [Reboot]({{< docsref "/content/reboot" >}}) 来纠正浏览器和设备之间的不一致，同时为常见的HTML元素提供稍微更有意义的重置。

## 社区

及时了解 Bootstrap 的开发情况，并通过这些有用的资源与社区联系。

- Follow [@getbootstrap on Twitter](https://twitter.com/{{< param twitter >}}).
- Read and subscribe to [The Official Bootstrap Blog]({{< param blog >}}/).
- Join [the official Slack room]({{< param slack >}}/).
- Chat with fellow Bootstrappers in IRC. On the `irc.freenode.net` server, in the `##bootstrap` channel.
- Implementation help may be found at Stack Overflow (tagged [`bootstrap-4`](https://stackoverflow.com/questions/tagged/bootstrap-4)).
- Developers should use the keyword `bootstrap` on packages which modify or add to the functionality of Bootstrap when distributing through [npm](https://www.npmjs.com/search?q=keywords:bootstrap) or similar delivery mechanisms for maximum discoverability.

你也可以关注 [@getbootstrap on Twitter](https://twitter.com/{{< param twitter >}}) 获取最新的八卦和精彩的音乐视频。
