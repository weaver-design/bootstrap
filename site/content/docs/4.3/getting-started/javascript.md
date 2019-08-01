---
layout: docs
title: JavaScript | 译：@GitHuboooSHY
description: 可选的 JavaScript 插件为 Bootstrap 注入活力。 了解熟悉每个插件，以及数据和编程 API 选项等。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 单独调用或整合调用

可以单独调用插件（使用 Bootstrap 单个的js文件  `js/dist/*.js`），也可以一次性使用 `bootstrap.js` 或压缩后的 `bootstrap.min.js` （不要同时调用两者）

`/js/dist/*.js` 文件已经 UMD 就绪，可以用于(Webpack, Rollup...)这样的打包工具。

## 以 module 的形式调用 Bootstrap

我们提供了一个构建为 `ESM`（`bootstrap.esm.js` 和 `bootstrap.esm.min.js`）的 Bootstrap 版本，它允许您在浏览器中使用 Bootstrap 作为 module，如果您的[目标浏览器支持它](https://caniuse.com/#feat=es6-module)。

{{< highlight html >}}
<script type="module">
  import { Toast } from 'bootstrap.esm.min.js'

  Array.from(document.querySelectorAll('.toast'))
    .forEach(toastNode => new Toast(toastNode))
</script>
{{< /highlight >}}

{{< callout warning >}}
## 不兼容的插件

由于浏览器的限制，我们的一些插件，即 Dropdown，Tooltip 和 Popover 插件，不能在带有 `module` 类型的 `<script>` 标签中使用，因为它们依赖于 Popper.js。 [了解更多](https://developers.google.com/web/fundamentals/primers/modules#specifiers).
{{< /callout >}}

## 插件依赖

一些插件和 CSS 组件依赖其他的插件。在独立地调用某个插件之前，请确认它所依赖的环境在文档中部署就绪。
 
例如我们的 dropdowns, popovers 和 tooltips 也取决于[ Popper.js](https://popper.js.org/)。

## Data 属性

几乎所有的 Bootstrap 插件都可以通过 HTML 单独使用 data 属性（我们使用 JavaScript 功能的首选方式）来启用和配置。确保在**单个元素上仅使用一组 data 属性**（例如，你无法在同一个 button 元素上触发 tooltip 和 modal 。）

{{< callout warning >}}
## 选择器

出于性能原因，一般我们使用原生函式 `querySelector` 和 `querySelectorAll` 去选择 DOM 元素，，因此你必须使用[有效的选择器](https://www.w3.org/TR/CSS21/syndata.html#value-def-identifier)。 如果使用特殊选择器，例如：`collapse:Example` 示例请务必转义它们。
{{< /callout >}}

## 事件

Bootstrap 为大多数插件的特有行为提供自定义事件。 通常，它们以不定式和过去的分词形式命名 - 在事件开始时触发不定式（例如 `show` ），并且在动作完成时触发其过去的分词形式（例如 `shown` ）。

所有不定式事件都提供 [`preventDefault()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault) 功能。 能在动作开始之前阻止它执行。 事件处理程序返回 false 也会自动调用 `preventDefault ()`。

{{< highlight js >}}
var myModal = document.getElementById('myModal')

myModal.addEventListener('show.bs.modal', function (e) {
  if (!data) {
    return e.preventDefault() // stops modal from being shown
  }
})
{{< /highlight >}}

## 编程 API

所有构造函数都可以有一个可选的选项对象或什么都没有（它启动一个具有默认行为的插件）：

{{< highlight js >}}
var myModalEl = document.getElementById('myModal')

var modal = new bootstrap.Modal(myModalEl) // initialized with defaults
var modal = new bootstrap.Modal(myModalEl, { keyboard: false }) // initialized with no keyboard
{{< /highlight >}}

如果您想获得特定的插件实例，每个插件都会公开一个 `_getInstance` 方法。 要直接从元素中检索它，请执行以下操作：`bootstrap.Popover._getInstance（myPopoverEl）`。

### 异步函数和转换

所有编程 API 方法都是**异步**的，在**转换结束前**返回给调用者。

为了在转换完成后立刻执行动作，你可以对相应的事件进行监听。

{{< highlight js >}}
var myCollapseEl = document.getElementById('#myCollapse')

myCollapseEl.addEventListener('shown.bs.collapse', function (e) {
  // Action to execute once the collapsible area is expanded
})
{{< /highlight >}}

此外，对**转换组件**的方法调用将无效。

{{< highlight js >}}
var myCarouselEl = document.getElementById('myCarousel')
var carousel = bootstrap.Carousel._getInstance(myCarouselEl) // Retrieve a Carousel instance

myCarouselEl.addEventListener('slid.bs.carousel', function (e) {
  carousel.to('2') // Will slide to the slide 2 as soon as the transition to slide 1 is finished
})

carousel.to('1') // Will start sliding to the slide 1 and returns to the caller
carousel.to('2') // !! Will be ignored, as the transition to the slide 1 is not finished !!
{{< /highlight >}}

### 默认设置

您可以通过修改插件的 `Constructor.Default` 对象来更改插件的默认设置：

{{< highlight js >}}
// changes default for the modal plugin's `keyboard` option to false
bootstrap.Modal.Default.keyboard = false
{{< /highlight >}}

## 冲突问题处理 (必须使用 jQuery)

有时需要将 Bootstrap 插件与其他 UI 框架一起使用。 在这些情况下，偶尔会发生命名空间冲突。 如果发生这种情况，您可以在要恢复值的插件上调用 `.noConflict`。

{{< highlight js >}}
var bootstrapButton = $.fn.button.noConflict() // return $.fn.button to previously assigned value
$.fn.bootstrapBtn = bootstrapButton // give $().bootstrapBtn the Bootstrap functionality
{{< /highlight >}}

## 版本号

可以通过插件构造函数的 `VERSION` 属性访问每个 Bootstrap 插件的版本。例如，对于工具提示插件：

{{< highlight js >}}
bootstrap.Tooltip.VERSION // => "{{< param current_version >}}"
{{< /highlight >}}

## 禁用 JavaScript 时没有特殊的 fallbacks

禁用 JavaScript 时，Bootstrap 的插件不会完美地抽离。如果您关心这种情况下的用户体验，请使用 [`<noscript>` ](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/noscript)向用户解释情况（以及如何重新启用JavaScript），亦或添加自己的自定义 fallbacks。

{{< callout warning >}}
##### 第三方库

**Bootstrap 不正式支持 Prototype 或 jQuery UI 等第三方 JavaScript 库**。 尽管 .noConflict 和命名空间事件，但您可能需要自行修复兼容性问题。{{< /callout >}}

## Sanitizer

Tooltips 和 Popovers 插件使用我们的内置 sanitizer 来清空接受 HTML 的选项。

默认的 `whiteList` 值如下：

{{< highlight js >}}
var ARIA_ATTRIBUTE_PATTERN = /^aria-[\w-]*$/i
var DefaultWhitelist = {
  // Global attributes allowed on any supplied element below.
  '*': ['class', 'dir', 'id', 'lang', 'role', ARIA_ATTRIBUTE_PATTERN],
  a: ['target', 'href', 'title', 'rel'],
  area: [],
  b: [],
  br: [],
  col: [],
  code: [],
  div: [],
  em: [],
  hr: [],
  h1: [],
  h2: [],
  h3: [],
  h4: [],
  h5: [],
  h6: [],
  i: [],
  img: ['src', 'alt', 'title', 'width', 'height'],
  li: [],
  ol: [],
  p: [],
  pre: [],
  s: [],
  small: [],
  span: [],
  sub: [],
  sup: [],
  strong: [],
  u: [],
  ul: []
}
{{< /highlight >}}

如果要向此默认 `whiteList` 添加新值，执行以下操作：

{{< highlight js >}}
var myDefaultWhiteList = bootstrap.Tooltip.Default.whiteList

// To allow table elements
myDefaultWhiteList.table = []

// To allow td elements and data-option attributes on td elements
myDefaultWhiteList.td = ['data-option']

// You can push your custom regex to validate your attributes.
// Be careful about your regular expressions being too lax
var myCustomRegex = /^data-my-app-[\w-]+/
myDefaultWhiteList['*'].push(myCustomRegex)
{{< /highlight >}}

如果你想使用专用库而不是我们的 sanitizer ，例如 `DOMPurify`，则应执行以下操作：

{{< highlight js >}}
var yourTooltipEl = document.getElementById('yourTooltip')
var tooltip = new bootstrap.Tooltip(yourTooltipEl, {
  sanitizeFn: function (content) {
    return DOMPurify.sanitize(content)
  }
})
{{< /highlight >}}
