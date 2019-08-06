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

在 Bootstrap 3 中，主题自定义包括 LESS 中的变量覆盖、自定义 CSS 以及我们包含在 `dist` 文件中的单独主题样式表。 由此我们可以很容易地将 Bootstrap 3 外观重新设计却不触及核心文件。 而 Bootstrap 4 提供了一种熟悉但略有不同的方法。

现在，主题定制由 Sass 变量，Sass 映射和自定义 CSS 完成。 现在，主题由 Sass 变量，Sass 映射和自定义 CSS 完成。 不再有专用的主题样式表; 取而代之的是启用内置主题来添加渐变，阴影等。

## Sass

通过源 Sass 文件来利用变量，贴图，混合等等。在我们的构建中，我们将 Sass 舍入精度提高到 6（默认为 5），以防止浏览器舍入问题。

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

#### Required keys

Bootstrap 假设 Sass 映射中存在一些特定的键，因为我们自己使用并扩展了这些键。 在自定义映射时，可能会遇到特定 Sass 映射的键被占用的报错。

For example, we use the `primary`, `success`, and `danger` keys from `$theme-colors` for links, buttons, and form states. Replacing the values of these keys should present no issues, but removing them may cause Sass compilation issues. In these instances, you'll need to modify the Sass code that makes use of those values.

### Functions

Bootstrap utilizes several Sass functions, but only a subset are applicable to general theming. We've included three functions for getting values from the color maps:

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

These allow you to pick one color from a Sass map much like how you'd use a color variable from v3.

{{< highlight scss >}}
.custom-element {
  color: gray("100");
  background-color: theme-color("dark");
}
{{< /highlight >}}

We also have another function for getting a particular _level_ of color from the `$theme-colors` map. Negative level values will lighten the color, while higher levels will darken.

{{< highlight scss >}}
@function theme-color-level($color-name: "primary", $level: 0) {
  $color: theme-color($color-name);
  $color-base: if($level > 0, #000, #fff);
  $level: abs($level);

  @return mix($color-base, $color, $level * $theme-color-interval);
}
{{< /highlight >}}

In practice, you'd call the function and pass in two parameters: the name of the color from `$theme-colors` (e.g., primary or danger) and a numeric level.

{{< highlight scss >}}
.custom-element {
  color: theme-color-level(primary, -10);
}
{{< /highlight >}}

Additional functions could be added in the future or your own custom Sass to create level functions for additional Sass maps, or even a generic one if you wanted to be more verbose.

### Color contrast

One additional function we include in Bootstrap is the color contrast function, `color-yiq`. It utilizes the [YIQ color space](https://en.wikipedia.org/wiki/YIQ) to automatically return a light (`#fff`) or dark (`#111`) contrast color based on the specified base color. This function is especially useful for mixins or loops where you're generating multiple classes.

For example, to generate color swatches from our `$theme-colors` map:

{{< highlight scss >}}
@each $color, $value in $theme-colors {
  .swatch-#{$color} {
    color: color-yiq($value);
  }
}
{{< /highlight >}}

It can also be used for one-off contrast needs:

{{< highlight scss >}}
.custom-element {
  color: color-yiq(#000); // returns `color: #fff`
}
{{< /highlight >}}

You can also specify a base color with our color map functions:

{{< highlight scss >}}
.custom-element {
  color: color-yiq(theme-color("dark")); // returns `color: #fff`
}
{{< /highlight >}}

## Sass options

Customize Bootstrap 4 with our built-in custom variables file and easily toggle global CSS preferences with new `$enable-*` Sass variables. Override a variable's value and recompile with `npm run test` as needed.

You can find and customize these variables for key global options in Bootstrap's `scss/_variables.scss` file.

| Variable                                     | Values                             | Description                                                                            |
| -------------------------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------- |
| `$spacer`                                    | `1rem` (default), or any value > 0 | Specifies the default spacer value to programmatically generate our [spacer utilities]({{< docsref "/utilities/spacing" >}}). |
| `$enable-rounded`                            | `true` (default) or `false`        | Enables predefined `border-radius` styles on various components. |
| `$enable-shadows`                            | `true` or `false` (default)        | Enables predefined `box-shadow` styles on various components. |
| `$enable-gradients`                          | `true` or `false` (default)        | Enables predefined gradients via `background-image` styles on various components. |
| `$enable-transitions`                        | `true` (default) or `false`        | Enables predefined `transition`s on various components. |
| `$enable-prefers-reduced-motion-media-query` | `true` (default) or `false`        | Enables the [`prefers-reduced-motion` media query]({{< docsref "/getting-started/accessibility#reduced-motion" >}}), which suppresses certain animations/transitions based on the users' browser/operating system preferences. |
| `$enable-hover-media-query`                  | `true` or `false` (default)        | **Deprecated** |
| `$enable-grid-classes`                       | `true` (default) or `false`        | Enables the generation of CSS classes for the grid system (e.g., `.container`, `.row`, `.col-md-1`, etc.). |
| `$enable-caret`                              | `true` (default) or `false`        | Enables pseudo element caret on `.dropdown-toggle`. |
| `$enable-pointer-cursor-for-buttons`         | `true` (default) or `false`        | Add "hand" cursor to non-disabled button elements. |
| `$enable-responsive-font-sizes`              | `true` or `false` (default)        | Enables [responsive font sizes]({{< docsref "/content/typography#responsive-font-sizes" >}}). |
| `$enable-validation-icons`                   | `true` (default) or `false`        | Enables `background-image` icons within textual inputs and some custom forms for validation states. |
| `$enable-deprecation-messages`               | `true` or `false` (default)        | Set to `true` to show warnings when using any of the deprecated mixins and functions that are planned to be removed in `v5`. |

## Color

Many of Bootstrap's various components and utilities are built through a series of colors defined in a Sass map. This map can be looped over in Sass to quickly generate a series of rulesets.

### All colors

All colors available in Bootstrap 4, are available as Sass variables and a Sass map in `scss/_variables.scss` file. This will be expanded upon in subsequent minor releases to add additional shades, much like the [grayscale palette](#grays) we already include.

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

Here's how you can use these in your Sass:

{{< highlight scss >}}
// With variable
.alpha { color: $purple; }

// From the Sass map with our `color()` function
.beta { color: color("purple"); }
{{< /highlight >}}

[Color utility classes]({{< docsref "/utilities/colors" >}}) are also available for setting `color` and `background-color`.

{{< callout info >}}
In the future, we'll aim to provide Sass maps and variables for shades of each color as we've done with the grayscale colors below.
{{< /callout >}}

### Theme colors

We use a subset of all colors to create a smaller color palette for generating color schemes, also available as Sass variables and a Sass map in Bootstraps's `scss/_variables.scss` file.

<div class="row">
  {{< theme-colors.inline >}}
  {{- range (index $.Site.Data "theme-colors") }}
    <div class="col-md-4">
      <div class="p-3 mb-3 swatch-{{ .name }}">{{ .name | title }}</div>
    </div>
  {{ end -}}
  {{< /theme-colors.inline >}}
</div>

### Grays

An expansive set of gray variables and a Sass map in `scss/_variables.scss` for consistent shades of gray across your project. Note that these are "cool grays", which tend towards a subtle blue tone, rather than neutral grays.

<div class="row mb-3">
  <div class="col-md-4">
    {{< theme-colors.inline >}}
    {{- range $.Site.Data.grays }}
      <div class="p-3 swatch-{{ .name }}">{{ .name }}</div>
    {{ end -}}
  {{< /theme-colors.inline >}}
  </div>
</div>

Within `scss/_variables.scss`, you'll find Bootstrap's color variables and Sass map. Here's an example of the `$colors` Sass map:

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

Add, remove, or modify values within the map to update how they're used in many other components. Unfortunately at this time, not _every_ component utilizes this Sass map. Future updates will strive to improve upon this. Until then, plan on making use of the `${color}` variables and this Sass map.

## Components

Many of Bootstrap's components and utilities are built with `@each` loops that iterate over a Sass map. This is especially helpful for generating variants of a component by our `$theme-colors` and creating responsive variants for each breakpoint. As you customize these Sass maps and recompile, you'll automatically see your changes reflected in these loops.

### Modifiers

Many of Bootstrap's components are built with a base-modifier class approach. This means the bulk of the styling is contained to a base class (e.g., `.btn`) while style variations are confined to modifier classes (e.g., `.btn-danger`). These modifier classes are built from the `$theme-colors` map to make customizing the number and name of our modifier classes.

Here are two examples of how we loop over the `$theme-colors` map to generate modifiers to the `.alert` component and all our `.bg-*` background utilities.

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

### Responsive

These Sass loops aren't limited to color maps, either. You can also generate responsive variations of your components or utilities. Take for example our responsive text alignment utilities where we mix an `@each` loop for the `$grid-breakpoints` Sass map with a media query include.

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

Should you need to modify your `$grid-breakpoints`, your changes will apply to all the loops iterating over that map.

## CSS variables

Bootstrap 4 includes around two dozen [CSS custom properties (variables)](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables) in its compiled CSS. These provide easy access to commonly used values like our theme colors, breakpoints, and primary font stacks when working in your browser's Inspector, a code sandbox, or general prototyping.

### Available variables

Here are the variables we include (note that the `:root` is required). They're located in our `_root.scss` file.

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

### Examples

CSS variables offer similar flexibility to Sass's variables, but without the need for compilation before being served to the browser. For example, here we're resetting our page's font and link styles with CSS variables.

{{< highlight css >}}
body {
  font: 1rem/1.5 var(--font-family-sans-serif);
}
a {
  color: var(--blue);
}
{{< /highlight >}}

### Breakpoint variables

While we originally included breakpoints in our CSS variables (e.g., `--breakpoint-md`), **these are not supported in media queries**, but they can still be used _within_ rulesets in media queries. These breakpoint variables remain in the compiled CSS for backward compatibility given they can be utilized by JavaScript. [Learn more in the spec](https://www.w3.org/TR/css-variables-1/#using-variables).

Here's an example of **what's not supported:**

{{< highlight css >}}
@media (min-width: var(--breakpoint-sm)) {
  ...
}
{{< /highlight >}}

And here's an example of **what is supported:**

{{< highlight css >}}
@media (min-width: 768px) {
  .custom-element {
    color: var(--primary);
  }
}
{{< /highlight >}}
