---
layout: docs
title: 浏览器和设备 | 译：@GitHuboooSHY
description: 了解Bootstrap支持的从现代到旧的浏览器和设备，包括每个浏览器和设备的已知怪癖和错误。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 支持的浏览器

Bootstrap 支持所有主流浏览器和平台的**最新稳定版本**。 在 Windows 上，**我们支持 Internet Explorer 11 / Microsoft Edge**。

使用最新版本的 WebKit，Blink 或 Gecko 的其他浏览器，无论直接或通过平台的 Web 访问 API，都未得到明确支持。 但是，Bootstrap 应该（在大多数情况下）在这些浏览器中正确显示和运行。 下面提供了更具体的支持信息。

您可以在我们的[ `.browserslistrc file` ]({{< param repo >}}/blob/v{{< param current_version >}}/.browserslistrc)上找到我们支持的浏览器及其版本 :

```text
{{< rf.inline >}}{{ readFile ".browserslistrc" | htmlEscape }}{{< /rf.inline >}}
```

我们使用 [Autoprefixer](https://github.com/postcss/autoprefixer) 通过 CSS 前缀处理预期的浏览器支持，CSS 前缀使用 [Browserslist](https://github.com/browserslist/browserslist) 来管理这些浏览器版本。有关如何将这些工具集成到项目中的信息，请参阅其文档

### 移动设备

一般来说，Bootstrap 支持每个主要平台的默认浏览器的最新版本。请注意，不支持代理浏览器（如 Opera Mini，Opera Mobile 的 Turbo 模式，UC Browser Mini，Amazon Silk）。

<table class="table table-bordered table-striped">
  <thead>
    <tr>
      <td></td>
      <th>Chrome</th>
      <th>Firefox</th>
      <th>Safari</th>
      <th>Android Browser &amp; WebView</th>
      <th>Microsoft Edge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Android</th>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
      <td class="text-muted">N/A</td>
      <td class="text-success">Android v5.0+ supported</td>
      <td class="text-success">Supported</td>
    </tr>
    <tr>
      <th scope="row">iOS</th>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
      <td class="text-muted">N/A</td>
      <td class="text-success">Supported</td>
    </tr>
    <tr>
      <th scope="row">Windows 10 Mobile</th>
      <td class="text-muted">N/A</td>
      <td class="text-muted">N/A</td>
      <td class="text-muted">N/A</td>
      <td class="text-muted">N/A</td>
      <td class="text-success">Supported</td>
    </tr>
  </tbody>
</table>

### 桌面端浏览器

同样，支持大多数桌面浏览器的最新版本。

<table class="table table-bordered table-striped">
  <thead>
    <tr>
      <td></td>
      <th>Chrome</th>
      <th>Firefox</th>
      <th>Internet Explorer</th>
      <th>Microsoft Edge</th>
      <th>Opera</th>
      <th>Safari</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Mac</th>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
      <td class="text-muted">N/A</td>
      <td class="text-muted">N/A</td>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
    </tr>
    <tr>
      <th scope="row">Windows</th>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported, IE11</td>
      <td class="text-success">Supported</td>
      <td class="text-success">Supported</td>
      <td class="text-danger">Not supported</td>
    </tr>
  </tbody>
</table>

对于 Firefox，除了最新的正常稳定版本外，我们还支持最新的 Firefox [扩展支持版本（ESR版本）](https://www.mozilla.org/en-US/firefox/organizations/#faq)。

当然，Bootstrap 应该在 Chromium 和 Chrome for Linux，Firefox for Linux 和 Internet Explorer 9 上完美运行，尽管它们不受官方支持。

有关Bootstrap必须解决的一些浏览器错误的列表，请参阅我们的 [Wall of browser bugs]({{< docsref "/browser-bugs" >}})。

## IE 浏览器

不支持 IE 11 以下的版本。请注意，IE 10 中不完全支持某些 CSS3 属性和 HTML5 元素，或者需要前缀属性才能获得完整功能。 访问 [Can I use...](https://caniuse.com/) 了解有关 CSS3 和 HTML5 功能的浏览器支持的详细信息。 **如果你要求支持 IE8-9, 请使用 Bootstrap 3.**

## 移动设备上的模态框和下拉菜单

### 溢出和滚动

支持 `overflow: hidden` ; `<body>` 元素在 iOS 和 Android 上非常有限。为此，当你在这些设备的浏览器中滑动经过模态框的顶部或底部时，`<body>` 内容将开始滚动。 请参阅 [Chrome bug #175502](https://bugs.chromium.org/p/chromium/issues/detail?id=175502) (fixed in Chrome v40) 和 [WebKit bug #153852](https://bugs.webkit.org/show_bug.cgi?id=153852).

### iOS文本字段和滚动

从iOS 9.2开始，当模态打开时，如果滚动手势的初始触摸位于文本 `<input>` 或 `<textarea>` 的边界内，则模态下面的 `<body>` 内容将滚动而不是模态本身。 See [WebKit bug #153856](https://bugs.webkit.org/show_bug.cgi?id=153856).

### 导航下拉菜单

由于 z-indexing 的复杂性，iOS 的导航不使用 `.dropdown-backdrop` 元素。因此，要关闭导航栏中的下拉列表，只能单击下拉元素或[将在iOS中触发单击事件的任何其他元素](https://developer.mozilla.org/en-US/docs/Web/Events/click#Safari_Mobile)。

## 浏览器缩放

页面缩放不可避免地在Bootstrap和Web的其余部分呈现某些组件中的渲染小部件。根据问题，我们可以修复它（首先搜索，然后在需要时打开问题）。 但是，我们倾向于忽略这些，因为除了黑客变通方法之外，它们通常没有直接的解决方案。

## 验证器

为了给旧的和会出现问题的浏览器提供最好的体验，Bootstrap 在几个地方使用 [ CSS browser hacks ](http://browserhacks.com/) 将特定的CSS定位到某些浏览器版本，以便解决浏览器本身的错误。通常这些 [ CSS browser hacks ](http://browserhacks.com/) 会被 CSS 验证器 声明是无效的。在一些地方，我们还使用尚未完全标准化的前沿 CSS 功能，但这些功能仅用于**渐进增强** （ 针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验 ）。

这些验证警告在实践中无关紧要，因为我们的 CSS 的非 hacky 部分确实完全验证并且 hacky 部分不会干扰非 hacky 部分的正常运行，因此我们故意忽略这些特定警告。

由于我们包含针对某个[ Firefox 错误 ](https://bugzilla.mozilla.org/show_bug.cgi?id=654072)的变通方法，因此我们的HTML文档同样具有一些微不足道且无关紧要的 HTML 验证警告。.
