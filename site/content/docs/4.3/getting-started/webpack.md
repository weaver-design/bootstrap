---
layout: docs
title: Webpack 和捆绑包 | 译：@GitHuboooSHY
description: 了解如何使用 Webpack 或其他捆绑包在项目中引入 Bootstrap。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 安装 Bootstrap

 使用 npm 将 [bootstrap 安装]({{< docsref "/getting-started/download#npm" >}})为 Node.js 模块

## 导入 JavaScript

通过将此行添加到应用程序的入口点（通常是 `index.js` 或 `app.js`）来导入[ Bootstrap 的 JavaScript]({{< docsref "/getting-started/javascript" >}})：

{{< highlight js >}}
// You can specify which plugins you need
import { Tooltip, Toast, Popover } from 'bootstrap';
{{< /highlight >}}

或者，如果你只需要我们的部分插件，可以根据需要**单独导入插件**：

{{< highlight js >}}
import Alert from 'bootstrap/js/dist/alert';
...
{{< /highlight >}}

Bootstrap 依赖于 [Popper](https://popper.js.org/)，它在 `peerDependencies` 属性中被指定。 这意味着您必须确保使用 `npm install popper.js` 将它们添加到 `package.json` 中。

## 导入样式

### 导入预编译的 Sass

要充分发挥 Bootstrap 的潜力并根据需要进行自定义，请将源文件用作项目捆绑过程的一部分。

首先，创建自己的 `_custom.scss` 并使用它来覆盖[内置的自定义变量]({{< docsref "/getting-started/theming" >}})。 然后，使用你的主 `Sass` 文件导入自定义变量，然后使用 Bootstrap：

{{< highlight scss >}}
@import "custom";
@import "~bootstrap/scss/bootstrap";
{{< /highlight >}}

要编译 Bootstrap，请确保安装并使用所需的加载器：[sass-loader](https://github.com/webpack-contrib/sass-loader)，带有 [Autoprefixer](https://github.com/postcss/autoprefixer#webpack) 的 [postcss-loader](https://github.com/postcss/postcss-loader)。 你的 webpack 配置应包括此规则或类似规则以实现最精简的设置：

{{< highlight js >}}
...
{
  test: /\.(scss)$/,
  use: [{
    loader: 'style-loader', // inject CSS to page
  }, {
    loader: 'css-loader', // translates CSS into CommonJS modules
  }, {
    loader: 'postcss-loader', // Run postcss actions
    options: {
      plugins: function () { // postcss plugins, can be exported to postcss.config.js
        return [
          require('autoprefixer')
        ];
      }
    }
  }, {
    loader: 'sass-loader' // compiles Sass to CSS
  }]
},
...
{{< /highlight >}}

### 导入编译的 CSS

或者，将此行添加到项目的入口点来使用Bootstrap的即用型CSS：

{{< highlight js >}}
import 'bootstrap/dist/css/bootstrap.min.css';
{{< /highlight >}}

在这种情况下，你可以使用现有的 `css` 规则，而无需对webpack配置进行任何特殊修改，除非你不需要 `sass-loader`，只用 [style-loader](https://github.com/webpack-contrib/style-loader) 和 [css-loader](https://github.com/webpack-contrib/css-loader) 。

{{< highlight js >}}
...
module: {
  rules: [
    {
      test: /\.css$/,
      use: ['style-loader', 'css-loader']
    }
  ]
}
...
{{< /highlight >}}
