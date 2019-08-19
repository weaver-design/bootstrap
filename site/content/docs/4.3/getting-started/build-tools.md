---
layout: docs
title: 构建工具 | 译：@GitHuboooSHY
description: 了解如何使用 Bootstrap 包含的 npm 脚本来构建我们的文档，编译源代码，运行测试等。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 工具设置

Bootstrap 使用 [npm脚本](https://docs.npmjs.com/misc/scripts) 构建系统。 我们的 [package.json]({{< param repo >}}/blob/v{{< param current_version >}}/package.json) 包含了使用框架的便捷方法，包括编译代码，运行测试等。

要使用我们的构建系统并在本地运行文档，你需要一份 Bootstrap 的源文件和 Node。 按照这些步骤来执行：

1. [下载并安装 Node.js](https://nodejs.org/en/download/)，用于管理我们的依赖项。
2. 导航到root `/bootstrap` 目录并运行 `npm install` 以安装 [package.json]({{< param repo >}}/blob/v{{< param current_version >}}/package.json) 中列出的本地依赖项。

完成后，就能够运行命令行提供的各种命令。

## 使用 npm 脚本

我们的 [package.json]({{< param repo >}}/blob/v{{< param current_version >}}/package.json) 包含以下命令和任务：

| Task | Description |
| --- | --- |
| `npm run dist` | `npm run dist` 为编译后的文件创建 `/dist/` 目录。 **使用 [Sass](https://sass-lang.com/)，[Autoprefixer][autoprefixer] 和 [terser](https://github.com/terser-js/terser)。** |
| `npm test` | 相当于 `npm run dist` plus，它在本地运行测试。 |
| `npm run docs` | 为文档构建和提示 CSS 和 JavaScript。 然后，你可以通过 `npm run docs-serve` 在本地运行文档。 |

运行 `npm run` 以查看所有 npm 脚本。

## Autoprefixer

Bootstrap 使用 [Autoprefixer][autoprefixer]（包含在我们的构建过程中）在构建时自动将供应商前缀添加到某些 CSS 属性。 这样做可以节省我们的时间和代码，允许我们一次编写 CSS 的关键部分，同时不需要像 v3 中那样的供应商混合。

我们通过 GitHub 存储库中的单独文件维护由 Autoprefixer 支持的浏览器列表。 有关详细信息，请参阅 [.browserslistrc]({{< param repo >}}/blob/v{{< param current_version >}}/.browserslistrc)。

## 本地文档

在本地运行我们的文档需要使用 Hugo，它通过 [hugo-bin](https://www.npmjs.com/package/hugo-bin) npm 包安装。 Hugo 是一个超快速且可扩展的静态站点生成器，它为我们提供：基本包含，基于 Markdown 的文件，模板等。 以下是如何开始：

1. 运行上面的[工具设置](#工具设置)以安装所有依赖项。
2. 从根目录 `/bootstrap` 的命令行中运行 `npm run docs-serve`。
3. 在浏览器中打开 `http://localhost:9001/`，然后瞧~

通过阅读 Hugo 的[文档](https://gohugo.io/documentation/)了解更多如何使用它的信息。

## 故障排除

如果您在安装依赖项时遇到问题，请卸载所有以前的依赖项版本（全局和本地）。 然后，重新运行 `npm install`。

[autoprefixer]: https://github.com/postcss/autoprefixer
