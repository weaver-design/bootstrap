---
layout: docs
title: 内容 | 译：@GitHuboooSHY
description: 了解Bootstrap中包含的内容，包括我们的预编译和源代码风格。
group: getting-started
aliases:
  - "/docs/4.3/getting-started/"
  - "/docs/getting-started/"
  - "/getting-started/"
toc: true
---

## 预编译的 Bootstrap

下载后，解压缩压缩文件夹，你会看到这样的东西：

<!-- NOTE: This info is intentionally duplicated in the README. Copy any changes made here over to the README too, but be sure to keep in mind to add the `dist` folder. -->

{{< highlight text >}}
bootstrap/
├── css/
│   ├── bootstrap-grid.css
│   ├── bootstrap-grid.css.map
│   ├── bootstrap-grid.min.css
│   ├── bootstrap-grid.min.css.map
│   ├── bootstrap-reboot.css
│   ├── bootstrap-reboot.css.map
│   ├── bootstrap-reboot.min.css
│   ├── bootstrap-reboot.min.css.map
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   └── bootstrap.min.css.map
└── js/
    ├── bootstrap.bundle.js
    ├── bootstrap.bundle.js.map
    ├── bootstrap.bundle.min.js
    ├── bootstrap.bundle.min.js.map
    ├── bootstrap.esm.js
    ├── bootstrap.esm.js.map
    ├── bootstrap.esm.min.js
    ├── bootstrap.esm.min.js.map
    ├── bootstrap.js
    ├── bootstrap.js.map
    ├── bootstrap.min.js
    └── bootstrap.min.js.map
{{< /highlight >}}

这是 Bootstrap 最基本的形式：预编译的文件，几乎可以在任何Web项目中快速使用。 我们提供编译好的 CSS 和 JS(`bootstrap.*`), 以及它们的压缩版本 (`bootstrap.min.*`)。[源映射](https://developers.google.com/web/tools/chrome-devtools/javascript/source-maps)（`bootstrap.* .map`）可用于某些浏览器的开发人员工具。捆绑的JS文件（`bootstrap.bundle.js` 和 `minified bootstrap.bundle.min.js`）包括[Popper](https://popper.js.org/)。

## CSS 文件

Bootstrap 包含一些选项，用于包含部分或全部编译的 CSS。

<table class="table table-bordered">
  <thead>
    <tr>
      <th scope="col">CSS files</th>
      <th scope="col">Layout</th>
      <th scope="col">Content</th>
      <th scope="col">Components</th>
      <th scope="col">Utilities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">
        <div><code class="font-weight-normal text-nowrap">bootstrap.css</code></div>
        <div><code class="font-weight-normal text-nowrap">bootstrap.min.css</code></div>
      </th>
      <td class="text-success">Included</td>
      <td class="text-success">Included</td>
      <td class="text-success">Included</td>
      <td class="text-success">Included</td>
    </tr>
    <tr>
      <th scope="row">
        <div><code class="font-weight-normal text-nowrap">bootstrap-grid.css</code></div>
        <div><code class="font-weight-normal text-nowrap">bootstrap-grid.min.css</code></div>
      </th>
      <td><a class="text-warning" href="{{< docsref "/layout/grid" >}}">Only grid system</a></td>
      <td class="bg-light text-muted">Not included</td>
      <td class="bg-light text-muted">Not included</td>
      <td><a class="text-warning" href="{{< docsref "/utilities/flex" >}}">Only flex utilities</a></td>
    </tr>
    <tr>
      <th scope="row">
        <div><code class="font-weight-normal text-nowrap">bootstrap-reboot.css</code></div>
        <div><code class="font-weight-normal text-nowrap">bootstrap-reboot.min.css</code></div>
      </th>
      <td class="bg-light text-muted">Not included</td>
      <td><a class="text-warning" href="{{< docsref "/content/reboot" >}}">Only Reboot</a></td>
      <td class="bg-light text-muted">Not included</td>
      <td class="bg-light text-muted">Not included</td>
    </tr>
  </tbody>
</table>

## JS 文件

同样，我们可以选择包含部分或全部已编译的 JavaScript。

<table class="table table-bordered">
  <thead>
    <tr>
      <th scope="col">JS files</th>
      <th scope="col">Popper</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">
        <div><code class="font-weight-normal text-nowrap">bootstrap.bundle.js</code></div>
        <div><code class="font-weight-normal text-nowrap">bootstrap.bundle.min.js</code></div>
      </th>
      <td class="text-success">Included</td>
    </tr>
    <tr>
      <th scope="row">
        <div><code class="font-weight-normal text-nowrap">bootstrap.js</code></div>
        <div><code class="font-weight-normal text-nowrap">bootstrap.min.js</code></div>
      </th>
      <td class="bg-light text-muted">Not included</td>
    </tr>
  </tbody>
</table>

## Bootstrap 源代码

Bootstrap 源代码下载包括预编译的 CSS 和 JavaScript 资产，以及源 Sass，JavaScript 和文档。 更具体地说，它包括以下内容：

{{< highlight text >}}
bootstrap/
├── dist/
│   ├── css/
│   └── js/
├── site/
│   └──content/
│      └── docs/
│          └── 4.3/
│              └── examples/
├── js/
└── scss/
{{< /highlight >}}

scss / 和js / 是我们的CSS和JavaScript的源代码。dist /文件夹包含上面预编译下载部分中列出的所有内容。site / docs / 文件夹包含我们文档的源代码和Bootstrap用法示例。除此之外，任何其他包含的文件都提供对文件包，许可证信息和开发的支持
