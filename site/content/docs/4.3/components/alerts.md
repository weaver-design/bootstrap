---
layout: docs
title: Alerts | 译：@GitHuboooSHY
description: 警告（Alerts）向用户提供了一种定义消息样式的方式。它们为典型的用户操作提供了上下文信息反馈。
group: components
aliases:
  - "/components/"
  - "/docs/4.3/components/"
toc: true
---

## 例

警告可以是任意长度的文本，并且你可以为它添加一个可选的关闭按钮。用给定的八个 **必须的** 上下文样式种类来给警告添加样式（例如`.alert-success`）。为了创建一个内联的可取消的警告框，请使用 警告（Alerts） jQuery 插件。

{{< example >}}
{{< alerts.inline >}}
{{- range (index $.Site.Data "theme-colors") }}
<div class="alert alert-{{ .name }}" role="alert">
  A simple {{ .name }} alert—check it out!
</div>{{- end -}}
{{< /alerts.inline >}}
{{< /example >}}

{{< callout info >}}
{{< partial "callout-warning-color-assistive-technologies.md" >}}
{{< /callout >}}

### 链接颜色

使用 `.alert-link` 实体类来快速提供带有匹配颜色的链接。

{{< example >}}
{{< alerts.inline >}}
{{- range (index $.Site.Data "theme-colors") }}
<div class="alert alert-{{ .name }}" role="alert">
  A simple {{ .name }} alert with <a href="#" class="alert-link">an example link</a>. Give it a click if you like.
</div>{{ end -}}
{{< /alerts.inline >}}
{{< /example >}}

### 附加内容

警告内部还可以添加例如标题，段落以及分割线这样的 HTML 元素。

{{< example >}}
<div class="alert alert-success" role="alert">
  <h4 class="alert-heading">Well done!</h4>
  <p>Aww yeah, you successfully read this important alert message. This example text is going to run a bit longer so that you can see how spacing within an alert works with this kind of content.</p>
  <hr>
  <p class="mb-0">Whenever you need to, be sure to use margin utilities to keep things nice and tidy.</p>
</div>
{{< /example >}}


### Dismissing

借助警告（alert）js插件，可以关闭内联警告。方法如下

- 确认已经安装警告（alert）插件或者已经引用 `Bootstrap JavaScript`。
- 创建一个关闭按钮，通过给警告添加  `.alert-dismissible` 类来给右方增加一些 `padding` 值，同时给按钮添加 `.close` 类来将其定位到警告栏中。
- 给关闭按钮添加 `data-dismiss="alert"` 属性以触发js功能。为了在各种设备上正常运行，确保使用 `<button>` 元素来创建按钮。
- 通过 `.fade` 和 `.show` 来给警告添加关闭/显示动画。

以下通过一个demo 来演示过程:

{{< example >}}
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
    <span aria-hidden="true">&times;</span>
  </button>
</div>
{{< /example >}}

## JavaScript 书写

### 触发器

启用关闭警告的原生js：

{{< highlight js >}}
var alertList = document.querySelectorAll('.alert')
alertList.forEach(function (alert) {
  new bootstrap.Alert(alert)
})
{{< /highlight >}}

或者**在警告栏中**添加 `data` 属性，如下所示：

{{< highlight html >}}
<button type="button" class="close" data-dismiss="alert" aria-label="Close">
  <span aria-hidden="true">&times;</span>
</button>
{{< /highlight >}}

注意关闭一个警告会将它从**文档**中移除。

### 方法

你可以使用警告构造函数来创建一个警告实例，例如：

{{< highlight js >}}
var myAlert = document.getElementById('myAlert')
var bsAlert = new bootstrap.Alert(myAlert)
{{< /highlight >}}

这一步使一个警告能够监听带有 `data-dismiss="alert"` 属性（当使用 data-api 中的 auto-initialization 时不需要）的后代元素所携带的点击事件。

| 方法 | 描述 |
| --- | --- |
| `close` | 关闭一个警告并且将其从文档中移除。如果该元素带有 `.fade` 和 `.show` 类，在被移除前将先执行动画。 |
| `dispose` | 销毁元素的警告。 |
| `_getInstance` | 静态方法，允许你获取与DOM元素关联的警报实例，例：`bootstrap.Alert._getInstance(alert)` |

{{< highlight js >}}
var alertNode = document.querySelector('.alert')
var alert = bootstrap.Alert._getInstance(alertNode)
alert.close()
{{< /highlight >}}

### 事件

Bootstrap 警告插件提供一些事件来为警报功能置入回调函数。

| Event | Description |
| --- | --- |
| `close.bs.alert` | 调用 `close` 实例方法时，此事件立即触发。 |
| `closed.bs.alert` | 警报关闭后将触发此事件（将等待 CSS 转换完成）。 |

{{< highlight js >}}
var myAlert = document.getElementById('myAlert')
myAlert.addEventListener('closed.bs.alert', function () {
  // do something…
})
{{< /highlight >}}
