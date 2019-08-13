---
layout: docs
title: 自定义 Bootstrap 主题 | 译：@GitHuboooSHY
description: 使用我们新的内置 Sass 变量自定义 Bootstrap 4，以获得全局样式首选项，以便于主题和组件更改。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 介绍

在 Bootstrap 3 中，主题自定义包括 LESS 中的变量覆盖、自定义 CSS 以及我们包含在 `dist` 文件中的单独主题样式表。 由此我们可以很容易地将 Bootstrap 3 外观重新设计却不触及核心文件。 而 Bootstrap 4 提供了一种略有不同的方法。

现在，主题定制由 Sass 变量，Sass 映射和自定义 CSS 完成。 不再有专用的主题样式表; 取而代之的是启用内置主题来添加渐变，阴影等。

## Sass

通过源 Sass 文件来利用变量，映射，混合等等。在我们的构建中，我们将 Sass 舍入精度提高到 6（默认为 5），以防止浏览器舍入问题。

### 文件结构

尽可能避免修改Bootstrap的核心文件。对于 Sass，这意味着创建自己的样式表，导入 Bootstrap，以便你可以修改和扩展它。 假设你正在使用像 npm 这样的包管理器，那么文件结构将如下所示：

{{< highlight text >}}
your-project/
├── scss
│   └── custom.scss
└── node_modules/
    └── bootstrap
        ├── js
        └── scss
{{< /highlight >}}

如果您已经下载了我们的源文件但没有使用包管理器，那么您需要手动设置类似于该结构的内容，使 Bootstrap 的源文件与您自己的源文件分开。

{{< highlight text >}}
your-project/
├── scss
│   └── custom.scss
└── bootstrap/
    ├── js
    └── scss
{{< /highlight >}}

### 导入

将 Bootstrap 的源 Sass 文件导入 `custom.scss` 中。你有两个选择：导入所有 Bootstrap，或只选择需要的部分。 我们鼓励后者，但要注意我们的组件有一些要求和依赖。 你还需要为我们的插件添加一些 JavaScript。

{{< highlight scss >}}
// Custom.scss
// Option A: Include all of Bootstrap

@import "../node_modules/bootstrap/scss/bootstrap";
{{< /highlight >}}

{{< highlight scss >}}
// Custom.scss
// Option B: Include parts of Bootstrap

// Required
@import "../node_modules/bootstrap/scss/functions";
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";

// Optional
@import "../node_modules/bootstrap/scss/reboot";
@import "../node_modules/bootstrap/scss/type";
@import "../node_modules/bootstrap/scss/images";
@import "../node_modules/bootstrap/scss/code";
@import "../node_modules/bootstrap/scss/grid";
{{< /highlight >}}

在此基础上，你就可以开始修改 `custom.scss` 中的任何 Sass 变量和映射。您也可以根据需要在 `// Optional` 部分下添加部分 Bootstrap。 我们建议使用 `bootstrap.scss` 文件中的完整导入堆栈作为起点。

### 变量默认值

Bootstrap 4 中的每个 Sass 变量都包含 `!default` 标志，允许你在不修改 Bootstrap 源代码的情况下覆盖自己 Sass 中变量的默认值。根据需要复制和粘贴变量，修改其值，并删除 `!default` 标志。 如果已经分配了变量，则不会通过 Bootstrap 中的默认值重新分配该变量。

你将在 `scss / _variables.scss` 中找到Bootstrap变量的完整列表。 某些变量设置为 `null` ，除非在配置中覆盖这些变量，否则这些变量不会输出该属性。

同一 Sass 文件中的变量覆盖可以在默认变量之前或之后。 但是，当覆盖 Sass 文件时，必须在导入 Bootstrap 的 Sass 文件之前进行覆盖。

下面是一个通过 npm 导入和编译 Bootstrap 后更改 `<body>` 的 `background-color` 和 `color` 的示例：

{{< highlight scss >}}
// Your variable overrides
$body-bg: #000;
$body-color: #111;

// Bootstrap and its default variables
@import "../node_modules/bootstrap/scss/bootstrap";
{{< /highlight >}}

根据需要对Bootstrap中的任何变量重复，包括下面的全局选项。

### 映射和循环

Bootstrap 4 包含一些更容易生成相关 CSS 系列的 Sass 映射、键值对。 我们使用 Sass 映射来定义颜色，网格断点等。 就像 Sass 变量一样，所有 Sass 映射都包含 `!default` 标志，可以被覆盖和扩展。

默认情况下，我们的一些 Sass 映射合并为空地图。 这样做是为了方便扩展给定的 Sass 映射，但是从地图中移除项目更麻烦了。

#### 修改映射

要修改 `$theme-colors` 映射中的现有颜色，请将以下内容添加到自定义 Sass 文件中：

{{< highlight scss >}}
$theme-colors: (
  "primary": #0074d9,
  "danger": #ff4136
);
{{< /highlight >}}

#### 添加到映射

添加新键和值来给 `$theme-colors` 添加新颜色：

{{< highlight scss >}}
$theme-colors: (
  "custom-color": #900
);
{{< /highlight >}}

#### 从映射中移除

要从 `$theme-colors` 或任何映射中移除 colors，请使用 `map-remove`。 注意必须在导入 sass 需求和 sass 选项之间插入它：

{{< highlight scss >}}
// Required
@import "../node_modules/bootstrap/scss/functions";
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";

$theme-colors: map-remove($theme-colors, "info", "light", "dark");

// Optional
@import "../node_modules/bootstrap/scss/root";
@import "../node_modules/bootstrap/scss/reboot";
@import "../node_modules/bootstrap/scss/type";
...
{{< /highlight >}}

#### 必要的键

Bootstrap 假设 Sass 映射中存在一些特定的键，因为我们自己使用并扩展了这些键。 在自定义映射时，可能会遇到特定 Sass 映射的键被占用的报错。

例如，我们使用 `$theme-colors` 中的 `primary` ， `success` 和 `danger` 键来表示链接，按钮和表单状态。 替换这些键的值没有问题，但删除它们可能会导致 Sass 编译问题。 在这些情况下，则需要修改使用这些值的 Sass 代码。

### 函数

Bootstrap 使用了几个 Sass 函数，但只有一个子集适用于一般主题。包含了三个从颜色映射中获取值的函数：

{{< highlight scss >}}
@function color($key: "blue") {
  @return map-get($colors, $key);
}

@function theme-color($key: "primary") {
  @return map-get($theme-colors, $key);
}

@function gray($key: "100") {
  @return map-get($grays, $key);
}
{{< /highlight >}}

这允许你从Sass映射中选择一种颜色，就像使用v3版本中的颜色变量一样。

{{< highlight scss >}}
.custom-element {
  color: gray("100");
  background-color: theme-color("dark");
}
{{< /highlight >}}

还有另一个函数，可以从 `$theme-colors` 映射中获取特定级别的颜色。 负值会提亮颜色，而值越大时颜色越暗。

{{< highlight scss >}}
@function theme-color-level($color-name: "primary", $level: 0) {
  $color: theme-color($color-name);
  $color-base: if($level > 0, #000, #fff);
  $level: abs($level);

  @return mix($color-base, $color, $level * $theme-color-interval);
}
{{< /highlight >}}

在实践中，调用该函数并传入两个参数： `$theme-colors`（例如，primary 或 danger ）的颜色名称和数字级别。

{{< highlight scss >}}
.custom-element {
  color: theme-color-level(primary, -10);
}
{{< /highlight >}}

在将来，可以添加其他函数或者自定义的 Sass 为其他 Sass 映射创建级别功能，如果你想要更详细，甚至可以添加一个通用函数。

### 颜色对比

在Bootstrap中包含另一个颜色对比函数 `color-yiq`。 此函数通过 [YIQ color space](https://en.wikipedia.org/wiki/YIQ)，根据指定的基色自动返回亮（`#fff`）或暗（`＃111`）的对比色。 对于生成多个类的混合或循环特别有用。

例如，从 `$theme-colors` 映射生成颜色样本：

{{< highlight scss >}}
@each $color, $value in $theme-colors {
  .swatch-#{$color} {
    color: color-yiq($value);
  }
}
{{< /highlight >}}

它还可用于一次性对比度需求：

{{< highlight scss >}}
.custom-element {
  color: color-yiq(#000); // returns `color: #fff`
}
{{< /highlight >}}

还可以使用我们的颜色映射函数指定基色：

{{< highlight scss >}}
.custom-element {
  color: color-yiq(theme-color("dark")); // returns `color: #fff`
}
{{< /highlight >}}

## Sass 选项

使用内置的自定义变量文件自定义 Bootstrap 4 主题，并通过新的 `$enable-*` Sass 变量轻松切换全局 CSS 首选项。 覆盖变量的值，并根据需要使用 `npm run test` 重新编译。

您可以在Bootstrap `scss/_variables.scss` 文件中找到并自定义这些变量以获取键的全局选项。

| 变量                                    | 值                             | 描述                                                                            |
| -------------------------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------- |
| `$spacer`                                    | `1rem` (default), or any value > 0 | 指定默认的 spacer 值，以编程方式生成 [spacer 实用程序]({{< docsref "/utilities/spacing" >}})。 |
| `$enable-rounded`                            | `true` (default) or `false`        | 在各种组件上启用预定义的 `border-radius` 样式。 |
| `$enable-shadows`                            | `true` or `false` (default)        | 在各种组件上启用预定义的 `box-shadow` 样式。 |
| `$enable-gradients`                          | `true` or `false` (default)        | 通过 `background-image` 样式在各种组件上启用预定义的渐变。 |
| `$enable-transitions`                        | `true` (default) or `false`        | 在各种组件上启用预定义的 `transition`。 |
| `$enable-prefers-reduced-motion-media-query` | `true` (default) or `false`        | 启用 [prefers-reduced-motion 媒体查询]({{< docsref "/getting-started/accessibility#reduced-motion" >}}) ，即抑制某些用户的浏览器/操作系统首选项自带的动画/转换。|
| `$enable-hover-media-query`                  | `true` or `false` (default)        | **弃用** |
| `$enable-grid-classes`                       | `true` (default) or `false`        | 允许为 grid 系统生成 CSS 类（例如，`.container`，`.row`，`.col-md-1` 等）。 |
| `$enable-caret`                              | `true` (default) or `false`        | 在 `.dropdown-toggle` 上启用伪元素插入符号。 |
| `$enable-pointer-cursor-for-buttons`         | `true` (default) or `false`        | 给非禁用按钮元素添加“手型”光标。 |
| `$enable-responsive-font-sizes`              | `true` or `false` (default)        | 启用[自适应字体大小]({{< docsref "/content/typography#responsive-font-sizes" >}})。  |
| `$enable-validation-icons`                   | `true` (default) or `false`        | 在文本输入和验证状态的自定义表单中启用 `background-image` 属性的图标。 |
| `$enable-deprecation-messages`               | `true` or `false` (default)        | 设置为 `true` 后，当使用在 `v5` 中将被移除的mixin和函数时显示警告 |

## 颜色

Bootstrap 的许多组件和实用程序的颜色都是通过 Sass 映射定义的。 此映射可以在 Sass 中循环以快速生成一系列规则集。

### 所有的颜色

Bootstrap 4 中提供的所有颜色都可用作 Sass 变量和 `scss/_variables.scss` 文件中的 Sass 映射。 这将在随后的维护性版本中进行扩展，以添加其他明暗程度的颜色，就像我们已经包含的[灰度调色板](#灰度)一样。

<div class="row">
  {{< theme-colors.inline >}}
  {{- range $.Site.Data.colors }}
    {{- if (and (not (eq .name "white")) (not (eq .name "gray")) (not (eq .name "gray-dark"))) }}
    <div class="col-md-4">
      <div class="p-3 mb-3 swatch-{{ .name }}">{{ .name | title }}</div>
    </div>
    {{ end -}}
  {{ end -}}
  {{< /theme-colors.inline >}}
</div>

以下是在Sass中使用这些内容的方法：

{{< highlight scss >}}
// With variable
.alpha { color: $purple; }

// From the Sass map with our `color()` function
.beta { color: color("purple"); }
{{< /highlight >}}

[颜色实用程序的类]({{< docsref "/utilities/colors" >}})也可用于设置字体颜色和背景颜色。

{{< callout info >}}
在未来，我们的目标是为每种颜色的明暗变化提供Sass贴图和变量，就像我们使用下面的灰度颜色一样。
{{< /callout >}}

### 主题颜色

我们给每个颜色一个子集来创建较小的调色板以生成颜色方案，也可以在 Bootstraps 的 `scss/_variables.scss` 文件中作为 Sass 变量和 Sass 映射使用。

<div class="row">
  {{< theme-colors.inline >}}
  {{- range (index $.Site.Data "theme-colors") }}
    <div class="col-md-4">
      <div class="p-3 mb-3 swatch-{{ .name }}">{{ .name | title }}</div>
    </div>
  {{ end -}}
  {{< /theme-colors.inline >}}
</div>

### 灰度

`scss/_variables.scss` 中的一组灰色变量和一个Sass映射，用于在项目中保持一致的灰度颜色。 请注意，这些是“冷灰色”，倾向于微妙的蓝色调，而不是中性灰色。

<div class="row mb-3">
  <div class="col-md-4">
    {{< theme-colors.inline >}}
    {{- range $.Site.Data.grays }}
      <div class="p-3 swatch-{{ .name }}">{{ .name }}</div>
    {{ end -}}
  {{< /theme-colors.inline >}}
  </div>
</div>

在 `scss/_variables.scss` 中，你将找到 Bootstrap 的颜色变量和 Sass 映射。 这是 `$colors` Sass映射的一个例子：

{{< highlight scss >}}
$colors: (
  "blue": $blue,
  "indigo": $indigo,
  "purple": $purple,
  "pink": $pink,
  "red": $red,
  "orange": $orange,
  "yellow": $yellow,
  "green": $green,
  "teal": $teal,
  "cyan": $cyan,
  "white": $white,
  "gray": $gray-600,
  "gray-dark": $gray-800
) !default;
{{< /highlight >}}

添加，删除或修改映射中的值，以更新它们在许多其他组件中的使用方式。但是，目前并非*所有*组件都使用此 Sass 映射。未来的更新将尽力改进这一点。 在此之前，计划使用 `${color}` 变量和这个 Sass 映射。

## 组件

Bootstrap 的许多组件和实用程序都是使用 `@each` 循环构建的，这些循环遍历 Sass 映射。 这对于通过 `$theme-colors` 生成组件的变体以及为每个断点创建响应变体特别有用。 当你自定义这些 Sass 地图并重新编译时，将看到这些更改在循环反映出来。

### 修饰符

Bootstrap 的许多组件都是使用 base-modifier 类方法构建的。 这意味着大部分样式包含在基类（例如`.btn`）中，而样式变化仅限于修饰符类（例如，`.btn-danger`）。 这些修饰符类是从 `$theme-colors` 映射构建的，用于自定义修饰符类的数量和名称。

以下两个示例展示我们如何遍历 `$theme-colors` 地图以生成 `.alert` 组件和所有 `.bg- *` 背景实用程序的修饰符。

{{< highlight scss >}}
// Generate alert modifier classes
@each $color, $value in $theme-colors {
  .alert-#{$color} {
    @include alert-variant(theme-color-level($color, -10), theme-color-level($color, -9), theme-color-level($color, 6));
  }
}

// Generate `.bg-*` color utilities
@each $color, $value in $theme-colors {
  @include bg-variant('.bg-#{$color}', $value);
}
{{< /highlight >}}

### 响应式

这些 Sass 循环也不仅限于颜色映射。 还可以生成组件或实用程序的响应式变体。 以我们的响应式文本对齐实用程序为例，我们将` $grid-breakpoints` Sass 映射的 `@each` 循环与媒体查询包括混合。

{{< highlight scss >}}
@each $breakpoint in map-keys($grid-breakpoints) {
  @include media-breakpoint-up($breakpoint) {
    $infix: breakpoint-infix($breakpoint, $grid-breakpoints);

    .text#{$infix}-left   { text-align: left !important; }
    .text#{$infix}-right  { text-align: right !important; }
    .text#{$infix}-center { text-align: center !important; }
  }
}
{{< /highlight >}}

如果你需要修改 `$grid-breakpoints`，更改结果将应用于迭代该映射的所有循环。

## CSS 变量

Bootstrap 4 在其编译的 CSS 中包含大约24个[ CSS 自定义属性（变量）](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)。 在浏览器的 Inspector，代码沙箱或一般原型设计中工作时，这些可以轻松访问常用值，如主题颜色，断点和主要字体堆栈。

### 可用变量

以下是我们包含的变量（请注意：`root` 是必需的）。 它们位于 `_root.scss` 文件中。

{{< highlight css >}}
:root {
  --blue: #007bff;
  --indigo: #6610f2;
  --purple: #6f42c1;
  --pink: #e83e8c;
  --red: #dc3545;
  --orange: #fd7e14;
  --yellow: #ffc107;
  --green: #28a745;
  --teal: #20c997;
  --cyan: #17a2b8;
  --white: #fff;
  --gray: #6c757d;
  --gray-dark: #343a40;
  --primary: #007bff;
  --secondary: #6c757d;
  --success: #28a745;
  --info: #17a2b8;
  --warning: #ffc107;
  --danger: #dc3545;
  --light: #f8f9fa;
  --dark: #343a40;
  --breakpoint-xs: 0;
  --breakpoint-sm: 576px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 992px;
  --breakpoint-xl: 1200px;
  --font-family-sans-serif: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  --font-family-monospace: SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
}
{{< /highlight >}}

### 示例

CSS 变量为 Sass 的变量提供了类似的灵活性，区别在它提供给浏览器之前无需编译。 例如，这里我们用 CSS 变量重置页面的字体和链接样式。

{{< highlight css >}}
body {
  font: 1rem/1.5 var(--font-family-sans-serif);
}
a {
  color: var(--blue);
}
{{< /highlight >}}

### 断点变量

虽然我们最初在 CSS 变量中包含断点（例如， `- burstpoint-md`），**而媒体查询中不支持这些断点**，不过它们仍可在媒体查询的规则集中使用。 这些断点变量保留在已编译的CSS中，以便向后兼容，因为它们可以被JavaScript使用。 [在规范中了解更多信息](https://www.w3.org/TR/css-variables-1/#using-variables)。

以下是**不支持**的示例：

{{< highlight css >}}
@media (min-width: var(--breakpoint-sm)) {
  ...
}
{{< /highlight >}}

以下是**支持**的示例：

{{< highlight css >}}
@media (min-width: 768px) {
  .custom-element {
    color: var(--primary);
  }
}
{{< /highlight >}}
