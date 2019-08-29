---
layout: docs
title: Accessibility | 译：@GitHuboooSHY
description: 简要概述 Bootstrap 创建可访问内容的功能和限制
group: getting-started
toc: true
---

Bootstrap 提供了一个易于使用的框架，包括现成样式，布局工具和交互式组件，允许开发人员创建具有视觉吸引力，功能丰富且可直接访问的网站和应用程序。

## 概述和限制

使用 Bootstrap 构建的任何项目的整体可访问性在很大程度上取决于作者的标记，其他样式以及它们包含的脚本。 但是，如果这些已经正确实现，那么使用 Bootstrap 创建满足 [<abbr title="Web Content Accessibility Guidelines">WCAG</abbr> 2.0](https://www.w3.org/TR/WCAG20/)（A/AA/AAA），[Section 508](https://www.section508.gov/) 以及类似可访问性标准和要求的网站和应用程序是完全可行的。

### 结构标记

Bootstrap 的样式和布局可以应用于各种标记结构。 本文档旨在为开发人员提供最佳实践示例，以演示 Bootstrap 本身的使用，并说明适当的语义标记，包括可以解决潜在可访问性问题的方法。

### 交互组件

Bootstrap 的交互式组件 - 例如模态对话框，下拉菜单和自定义工具提示 - 旨在服务触摸，鼠标和键盘用户。 通过使用相关的 [<abbr title="Web Accessibility Initiative">WAI</abbr>-<abbr title="Accessible Rich Internet Applications">ARIA</abbr>](https://www.w3.org/WAI/standards-guidelines/aria/) 角色和属性，这些组件也应该是可理解的，并且可以使用辅助技术（例如屏幕阅读器）进行操作。

由于 Bootstrap 的组件专门设计为相当通用，因此作者可能需要包含更多 <abbr title="Accessible Rich Internet Applications">ARIA</abbr>角色和属性以及 JavaScript 行为，以更准确地传达其组件的精确性质和功能。 这通常在文档中注明。

### 色彩对比

当前 Bootstrap 的默认调色板（在整个框架中用于按钮变化，警报变化，形状验证指示器）中的大部分颜色在使用浅色背景时 *缺乏* 颜色对比度（低于建议的[ WCAG 2.0 颜色对比度 4.5:1](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html)） 。 作者需要手动修改或扩展这些默认颜色，以确保足够的颜色对比度。

### 视觉隐藏的内容

视觉上隐藏但仍可供屏幕阅读器等辅助技术访问的内容，给予 `.sr-only` 类进行样式设置。 这在其他视觉信息或提示（例如通过使用颜色表示的含义）也需要传达给非视觉用户的情况下非常有用。

{{< highlight html >}}
<p class="text-danger">
  <span class="sr-only">Danger: </span>
  This action is not reversible
</p>
{{< /highlight >}}

对于视觉上隐藏的交互式控件，例如传统的“跳过”链接，请使用 `.sr-only-focusable` 类。 这将确保控制在聚焦后变得可见（对于有视力的键盘用户）。**注意，从 Bootstrap 5 开始，`.sr-only-focusable` 类不能与 `.sr-only` 类结合使用**。

{{< highlight html >}}
<a class="sr-only-focusable" href="#content">Skip to main content</a>
{{< /highlight >}}

### 减少动作

Bootstrap 包括对 [`prefers-reduced-motion` 媒体功能](https://drafts.csswg.org/mediaqueries-5/#prefers-reduced-motion)的支持。 在允许用户指定减少运动的首选项的浏览器/环境中，Bootstrap 中的大多数 CSS 过渡效果（例如，打开或关闭模式对话框时，或轮播中的滑动动画）将被禁用。

## 资源链接

- [Web Content Accessibility Guidelines (WCAG) 2.0](https://www.w3.org/TR/WCAG20/)
- [The A11Y Project](https://a11yproject.com/)
- [MDN accessibility documentation](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
- [Tenon.io Accessibility Checker](https://tenon.io/)
- [Colour Contrast Analyser (CCA)](https://developer.paciellogroup.com/resources/contrastanalyser/)
- ["HTML Codesniffer" bookmarklet for identifying accessibility issues](https://github.com/squizlabs/HTML_CodeSniffer)
