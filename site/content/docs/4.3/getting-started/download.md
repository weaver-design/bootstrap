---
layout: docs
title: 下载 | 译：@Turkyden
description: 下载 Bootstrap 以获取已编译的 CSS 和 JavaScript，源代码，或将其包含在您最喜欢的包管理器中，如 npm，RubyGems 等。
group: getting-started
toc: true
---

## 已编译 CSS 和 JS

只下载已编译好的 **Bootstrap v{{< param current_version >}}**代码，直接引入你的项目就能使用，这里面包括：

- 已编译和压缩的 CSS 包（参见 [CSS 文件对照表]({{< docsref "/getting-started/contents#css-files" >}})）
- 已编译和压缩的 JavaScript 插件

这里面不包括文档，源代码，或者任何 JavaScript 依赖包比如 Popper.js。

<a href="{{< param "download.dist" >}}" class="btn btn-bd-primary" onclick="ga('send', 'event', 'Getting started', 'Download', 'Download Bootstrap');">快速下载</a>

## 源代码

通过下载我们的源码包括 Sass，JavaScript 和文档文件，使用您自己的资产管道编译 Bootstrap。此选项需要一些额外的工具：

- 用于编译 CSS 的 Sass 编译器（支持 Libsass 或 Ruby Sass）
- 用于为 CSS 添加前缀的插件 [Autoprefixer](https://github.com/postcss/autoprefixer)

如果您需要[构建工具]({{< docsref "/getting-started/build-tools#tooling-setup" >}})，它们包含在开发 Bootstrap 及其文档中，但它们可能不一定符合您的心意。

<a href="{{< param "download.source" >}}" class="btn btn-bd-primary" onclick="ga('send', 'event', 'Getting started', 'Download', 'Download source');">下载源码</a>

## BootstrapCDN

跳过下载，使用 [BootstrapCDN](https://www.bootstrapcdn.com/) 将 Bootstrap 的已编译 CSS 和 JS 的缓存版本加入你的项目。

{{< highlight html >}}
<link rel="stylesheet" href="{{< param "cdn.css" >}}" integrity="{{< param "cdn.css_hash" >}}" crossorigin="anonymous">
<script src="{{< param "cdn.js" >}}" integrity="{{< param "cdn.js_hash" >}}" crossorigin="anonymous"></script>
{{< /highlight >}}

如果您正在使用我们编译的 JavaScript，请不要忘记在我们的JS之前通过 CDN 优先包含 Popper.js。

{{< highlight html >}}
<script src="{{< param "cdn.popper" >}}" integrity="{{< param "cdn.popper_hash" >}}" crossorigin="anonymous"></script>
{{< /highlight >}}

## 包管理工具

将 Bootstrap 的**源代码**引入到几乎所有项目中，其中包含一些最受欢迎的软件包管理器。无论什么包管理器，Bootstrap 都需要一个 **Sass 编译器**和 [Autoprefixer](https://github.com/postcss/autoprefixer) 来进行与我们官方编译版本相匹配的设置。

### npm

在 Node.js 驱动的应用程序中，使用 [npm](https://www.npmjs.com/package/bootstrap) 包安装 Bootstrap：

{{< highlight sh >}}
npm install bootstrap
{{< /highlight >}}

`const bootstrap = require('bootstrap')` 或者 `import bootstrap from 'bootstrap'` 将加载 Bootstrap 中所有的插件到一个 `bootstrap` 对象上。这个 `bootstrap` 模块本身已经导出了所有的插件。您可以通过在程序包的顶级目录下加载 `/js/dist/*.js` 文件来单独手动加载 Bootstrap 的插件。

Bootstrap 的 `package.json` 中包含了以下关键的一些其他元信息：

- `sass` - Bootstrap 的 [Sass](https://sass-lang.com/) 文件主要入口路径
- `style` - Bootstrap 使用默认设置预编译的非压缩 CSS 的路径 (未定制)

### yarn

在你 Node.js 驱动的应用程序中使用 [yarn 包管理](https://yarnpkg.com/en/package/bootstrap) 安装 Bootstrap：

{{< highlight sh >}}
yarn add bootstrap
{{< /highlight >}}

### RubyGems

在你的 Ruby 应用中使用 [Bundler](https://bundler.io/) (**推荐**) 和 [RubyGems](https://rubygems.org/) 下载 Bootstrap，请在你的 [`Gemfile`](https://bundler.io/gemfile.html) 中添加以下：

{{< highlight ruby >}}
gem 'bootstrap', '~> {{< param current_ruby_version >}}'
{{< /highlight >}}

或者，如果您不使用Bundler，则可以通过运行以下命令来安装gem：

{{< highlight sh >}}
gem install bootstrap -v {{< param current_ruby_version >}}
{{< /highlight >}}

更多内容请查阅 [gem README](https://github.com/twbs/bootstrap-rubygem/blob/master/README.md) 说明文档。

### Composer

您还可以使用 [Composer](https://getcomposer.org/) 安装和管理 Bootstrap 的 Sass 和 JavaScript：

{{< highlight sh >}}
composer require twbs/bootstrap:{{< param current_version >}}
{{< /highlight >}}

### NuGet

如果您使用 .NET 开发，您还可以使用 [NuGet](https://www.nuget.org/) 安装和管理 Bootstrap 的 [CSS](https://www.nuget.org/packages/bootstrap/) 或 [Sass](https://www.nuget.org/packages/bootstrap.sass/) 和 JavaScript：

{{< highlight powershell >}}
Install-Package bootstrap
{{< /highlight >}}

{{< highlight powershell >}}
Install-Package bootstrap.sass
{{< /highlight >}}
